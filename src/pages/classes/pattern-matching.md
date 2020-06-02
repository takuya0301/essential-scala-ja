## パターンマッチング

これまでは、メソッドを呼び出したり、フィールドにアクセスしたりしてオブジェクトを作用させてきました。ケースクラスを使用すると、**パターンマッチング (pattern matching)** を通じた別の方法で作用させることができます。

パターンマッチングは、データの「形」に応じて式を評価できるように拡張した `if` 式のようなものです。前の例で見た `Person` ケースクラスを思い出してください。

```tut:book:silent
case class Person(firstName: String, lastName: String)
```

反乱軍のメンバーを探している `Stormtrooper` を実装したいとしましょう。下記のようにパターンマッチングを使用することができます。

```tut:book:silent
object Stormtrooper {
  def inspect(person: Person): String =
    person match {
      case Person("Luke", "Skywalker") => "Stop, rebel scum!"
      case Person("Han", "Solo") => "Stop, rebel scum!"
      case Person(first, last) => s"Move along, $first"
    }
}
```

パターンの文法 `Person("Luke", "Skywalker")` は、パターンマッチングするオブジェクトを構築するための文法 `Person("Luke", "Skywalker")` と一致していることに注意してください。

使用例はこちらです。

```tut:book
Stormtrooper.inspect(Person("Noel", "Welsh"))
Stormtrooper.inspect(Person("Han", "Solo"))
```

<div class="callout callout-info">
#### パターンマッチング文法 {-}

パターンマッチング式の文法は、

```scala
expr0 match {
  case pattern1 => expr1
  case pattern2 => expr2
  ...
}
```

です。ここで、

- `expr0` はパターンマッチングする値に評価される式
- `pattern1`、`pattern2` などは値に対して順番に検証されるパターンもしくは**ガード (guard)**
- `expr1`、`expr2` などは式で、最初にマッチングしたパターンの右辺式が評価される[^compilation]

とします。パターンマッチング自体は式であるため、マッチングしたパターンの右辺式の値に評価されます。
</div>

[^compilation]: 実際のところ、パターンは逐次的な検証より効率的な形にコンパイルされますが、それが意味するところは変わりません。


### パターン文法

Scala は、パターンやガードを記述するための表現力豊かな文法を持っています。ケースクラスの場合、パターン文法はコンストラクター文法と一致します。データを取り上げてみましょう。

```tut:book
Person("Noel", "Welsh")
```

`Person` 型に対してマッチするパターンは、下記のように記述されます。

```scala
Person(pat0, pat1)
```

ここで、`pat0` と `pat1` は、それぞれ `firstName` と `lastName` に対してマッチするパターンです。`pat0` と `pat1` の位置に使用できるパターンは4つあります。

1. 名前。それは、その位置にある任意の値にマッチし、与えられた名前に束縛されます。例えば、パターン `Person(first, last)` は、名前 `first` に値 `"Noel"` を、名前 `last` に値 `"Welsh"` を束縛します。

2. アンダースコア (`_`)。それは、任意の値にマッチし、その値を無視します。例えば、ストームトルーパーが一般市民の名についてのみ気にするのであれば、`lastName` の値を名前に束縛することを避け、単に `Person(first, _)` と書くことができます。

3. リテラル。それは、単にリテラルが表現する値に首尾よくマッチします。例えば、パターン `Person("Han", "Solo")` は、名が `"Han"` で、姓が `"Solo"` である `Person` にマッチするというわけです。

4. 同様のコンストラクタースタイル文法を使用している別のケースクラス。（訳注：ケースクラスを入れ子にできるということです。本節の演習に例があります。）

パターンマッチングでできることは他にもたくさんあり、パターンマッチングは拡張可能であることも覚えておいてください。後ほどの節でそれらの機能を見ていきます。


### 覚えておいてほしいこと

ケースクラスは、**パターンマッチング**と呼ばれる相互作用の新しい形を可能にします。パターンマッチングでは、ケースクラスによって分析し、ケースクラスに含まれる内容に応じて異なる式を評価することができます。

パターンマッチングのための文法は、

```scala
expr0 match {
  case pattern1 => expr1
  case pattern2 => expr2
  ...
}
```

です。パターンは、下記のいずれかになります。

1. 名前。その名前に任意の値が束縛される
2. アンダースコア。任意の値がマッチし、その値は無視される
3. リテラル。リテラルが意味する値にマッチする
4. ケースクラスによるコンストラクタースタイルパターン

### 演習

#### 猫にエサをあげる

`willServe` メソッドを伴う `ChipShop` オブジェクトを定義してください。このメソッドは `Cat` を受け取り、猫の好きなエサがカリカリ (chips) であれば `true` を、そうでなければ `false` を返します。パターンマッチングを使用してください。

<div class="solution">
問題文が示唆しているスケルトンを記述することから始めましょう。

```tut:book:silent
case class Cat(name: String, colour: String, food: String)
```

```scala
object ChipShop {
  def willServe(cat: Cat): Boolean =
    cat match {
      case Cat(???, ???, ???) => ???
    }
}
```

返却型は `Boolean` なので、少なくとも2つのケース、ひとつは `true`、もうひとつは `false` が必要であることがわかります。演習の文章は、それがカリカリが好きな猫とそれ以外の猫であることを示しています。これをリテラルパターンと `_` パターンで実装することができます。

```tut:book:silent
object ChipShop {
  def willServe(cat: Cat): Boolean =
    cat match {
      case Cat(_, _, "Chips") => true
      case Cat(_, _, _) => false
    }
}
```
</div>


#### Get Off My Lawn!

In this exercise we're going to write a simulator of my Dad, the movie critic. It's quite simple: any movie directed by Clint Eastwood gets a rating 10.0, any movie directed by John McTiernan gets a 7.0, while any other movie gets a 3.0. Implement an object called `Dad` with a method `rate` which accepts a `Film` and returns a `Double`. Use pattern matching.

<div class="solution">
```scala
object Dad {
  def rate(film: Film): Double =
    film match {
      case Film(_, _, _, Director("Clint", "Eastwood", _)) => 10.0
      case Film(_, _, _, Director("John", "McTiernan", _)) => 7.0
      case _ => 3.0
    }
}
```

Pattern matching is becoming quite verbose in this case. Later on we'll learn how we can use pattern matching to match a particular value, called a *constant pattern*.
</div>
