## コンパニオンオブジェクト

論理的にはクラスに所属していても、どの特定のオブジェクトからも独立しているメソッドを作成したいことがあります。そのために Java では**静的メソッド (static method)** を使用しますが、Scala にはシングルトンオブジェクトというもっと単純な解決策があります。

一般的な使用例は補助コンストラクターです。Scala がクラスについて複数のコンストラクターを定義できる文法を持つにも関わらず、Scala プログラマーはほとんどの場合、クラスと同じ名前を持つオブジェクトに、追加のコンストラクターとして `apply` メソッドを実装することを好みます。そのオブジェクトをクラスの**コンパニオンオブジェクト (companion object)** と呼びます。例えば、下記のようになります。

```tut:book:silent
class Timestamp(val seconds: Long)

object Timestamp {
  def apply(hours: Int, minutes: Int, seconds: Int): Timestamp =
    new Timestamp(hours*60*60 + minutes*60 + seconds)
}
```

```tut:book
Timestamp(1, 1, 1).seconds
```

<div class="callout callout-info">
#### コンソールを有効活用する {-}

上記の例は、`:paste` コマンドを使用することに注意してください。コンパニオンオブジェクトは、後援するクラスと同じコンパイル単位で定義されなければなりません。通常のコードベースで、これはクラスとオブジェクトを同じファイルの中に定義することを意味しますが、REPL 上では `:paste` を使用し、それらをひとつのコマンドとして入力しなければなりません。

REPL 上に `:help` と入力することでより詳細を知ることができます。
</div>

前述のように、Scala は、**型名 (type name)** 空間と**値名 (value name)** 空間の2つの名前空間を持ちます。このように分離することで、衝突なくクラスとコンパニオンオブジェクトに同じ名前を付けることができます。

シングルトンオブジェクトは独自の型を伴うので、**コンパニオンオブジェクトはクラスのインスタンスではない**という重要なことに注意してください。

```tut:book
Timestamp // 型は `Timestamp.type` であり `Timestamp` ではないことに注意
```

<div class="callout callout-info">
#### コンパニオンオブジェクト文法 {-}

クラスのためにコンパニオンオブジェクトを定義するには、同じ名前のオブジェクトをクラスと**同じファイル**に定義します。

```scala
class Name {
  ...
}

object Name {
  ...
}
```
</div>

### 覚えておいてほしいこと

**コンパニオンオブジェクト**は、機能をクラスのインスタンスに関連付けることなく、クラスに関連付ける手段を提供します。一般的に、それらは追加のコンストラクターを提供するために使用されます。

コンパニオンオブジェクトは **Java の静的メソッドを代替します**。それらは等価な機能を提供し、より柔軟です。

**コンパニオンオブジェクトは、関連付けられたクラスと同じ名前を持ちます。** これは、値の名前空間と型の名前空間という2つの名前空間を Scala が持つため、名前の衝突を引き起こしません。

**コンパニオンオブジェクトは、関連付けられたクラスと同じファイルに定義されなければいけません。** REPL 上で入力するときは `:paste` モードを使用して、クラスとコンパニオンオブジェクトが同じコードブロックで入力されなければいけません。

### 演習

#### フレンドリーな人ファクトリ

姓と名を個別にではなく、姓名全体をひとつの文字列として受け取る `apply` メソッドを含む、`Person` のコンパニオンオブジェクトを実装してください。

ヒント：下記のようにして `String` を `Array` の要素に分割できます。

```tut:book
val parts = "John Doe".split(" ")
parts(0)
```

<div class="solution">
こちらがコードです。

```tut:book:silent
class Person(val firstName: String, val lastName: String) {
  def name: String =
    s"$firstName $lastName"
}

object Person {
  def apply(name: String): Person = {
    val parts = name.split(" ")
    new Person(parts(0), parts(1))
  }
}
```

こちらが使用例です。

```tut:book
Person.apply("John Doe").firstName // 完全なメソッド呼び出し
Person("John Doe").firstName // 糖衣適用文法
```
</div>

#### 業績の派生的な内容

下記のように、`Director` と `Film` のためのコンパニオンオブジェクトを記述してください。

 - `Director` コンパニオンオブジェクトに含まれる：
    - クラスのコンストラクターと同じ引数を受け取り、新しい `Director` を返す `apply` メソッド
    - 2つの `Director` を受け取り、2つのうち年齢が上の方を返す `older` メソッド

 - `Film` コンパニオンオブジェクトに含まれる：
    - クラスのコンストラクターと同じ引数を受け取り、新しい `Film` を返す `apply` メソッド
    - 2つの `Film` を受け取り、2つのうち `imdbRating` が高い方を返す `highestRating` メソッド
    - 2つの `Film` を受け取り、それぞれの撮影時に年齢が上の `Director` を返す`oldestDirectorAtTheTime` メソッド

<div class="solution">

この演習は、たくさんのコードを書く練習を提供することを意図しています。前節のクラス定義を含む模範回答はこのようになります。

```tut:book:silent
class Director(
  val firstName: String,
  val lastName: String,
  val yearOfBirth: Int) {

  def name: String =
    s"$firstName $lastName"

  def copy(
    firstName: String = this.firstName,
    lastName: String = this.lastName,
    yearOfBirth: Int = this.yearOfBirth) =
    new Director(firstName, lastName, yearOfBirth)
}

object Director {
  def apply(firstName: String, lastName: String, yearOfBirth: Int): Director =
    new Director(firstName, lastName, yearOfBirth)

  def older(director1: Director, director2: Director): Director =
    if (director1.yearOfBirth < director2.yearOfBirth) director1 else director2
}

class Film(
  val name: String,
  val yearOfRelease: Int,
  val imdbRating: Double,
  val director: Director) {

  def directorsAge =
    director.yearOfBirth - yearOfRelease

  def isDirectedBy(director: Director) =
    this.director == director

  def copy(
    name: String = this.name,
    yearOfRelease: Int = this.yearOfRelease,
    imdbRating: Double = this.imdbRating,
    director: Director = this.director) =
    new Film(name, yearOfRelease, imdbRating, director)
}

object Film {
  def apply(
    name: String,
    yearOfRelease: Int,
    imdbRating: Double,
    director: Director): Film =
    new Film(name, yearOfRelease, imdbRating, director)

  def newer(film1: Film, film2: Film): Film =
    if (film1.yearOfRelease < film2.yearOfRelease) film1 else film2

  def highestRating(film1: Film, film2: Film): Double = {
    val rating1 = film1.imdbRating
    val rating2 = film2.imdbRating
    if (rating1 > rating2) rating1 else rating2
  }

  def oldestDirectorAtTheTime(film1: Film, film2: Film): Director =
    if (film1.directorsAge > film2.directorsAge) film1.director else film2.director
}
```

</div>

#### 型か値か？

クラスとコンパニオンオブジェクトの名前付けによる類似は、新しい Scala 開発者の混乱を引き起こしがちです。コードのブロックを読んでいるとき、その部分がクラス（**型**）を示しているのか、シングルトンオブジェクト（**値**）を示しているのかについて知っていることが重要です。

**「型か値か？」** という新しくヒットしそうなクイズを思いついたので、これから試してみましょう。単語 `Film` の参照しているものが型か値かをそれぞれの事例で特定してください。

```scala
val prestige: Film = bestFilmByChristopherNolan()
```

<div class="solution">
**型！**このコードは、型 `Film` の値 `prestige` を定義しています。
</div>

```scala
new Film("Last Action Hero", 1993, mcTiernan)
```

<div class="solution">
**型！**これは、`Film` の**コンストラクター**への参照です。このコンストラクターは、**型**である**クラス** `Film` の一部です。
</div>

```scala
Film("Last Action Hero", 1993, mcTiernan)
```

<div class="solution">
**値！**これは、下記の略記法です。

```scala
Film.apply("Last Action Hero", 1993, mcTiernan)
```

`apply` は、**シングルトンオブジェクト**（値）`Film` に定義されたメソッドです。
</div>

```scala
Film.newer(highPlainsDrifter, thomasCrownAffair)
```

<div class="solution">
**値！**`newer` は、**シングルトンオブジェクト** `Film` に定義された別のメソッドです。
</div>

最後は難しいものを……

```scala
Film.type
```

<div class="solution">
**値！**これは巧妙です！これを間違えたとしても許されるでしょう。

`Film.type` は、シングルトンオブジェクト `Film` の型を参照するので、この場合の `Film` は値への参照です。しかしながら、コードの断片全体は型になります。
</div>
