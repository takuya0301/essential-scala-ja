## ケースクラス

**ケースクラス (case class)** は、クラスやコンパニオンオブジェクト、たくさんの実用的なデフォルト機能をまとめて定義してくれる非常に便利な略記法です。それは、やっかいごとを最小限にして、軽量なデータ保持クラスを生成するための理想的な方法になります。

ケースクラスは、クラス定義にキーワード `case` を前置するだけで生成されます。

```tut:book:silent
case class Person(firstName: String, lastName: String) {
  def name = firstName + " " + lastName
}
```

ケースクラスを定義するとき、いつでも Scala は自動的に**クラスとコンパニオンオブジェクト**を生成します。

```tut:book
val dave = new Person("Dave", "Gurnell") // クラスを持つ
Person // コンパニオンオブジェクトも
```

さらに、クラスとコンパニオンオブジェクトには、とても便利ないくつかの機能があらかじめ用意されています。

### ケースクラスの機能

1. **各コンストラクター引数のフィールド**。コンストラクター定義に `val` と書く必要はありませんし、明示的に書いても悪影響はありません。

```tut:book
dave.firstName
```

2. **デフォルト `toString` メソッド**。それは、クラスのコンストラクターに似た実用的な表現を印字します。もう `@` 記号と謎めいた16進数とはおさらばです。

```tut:book
dave
```

3. **実用的な `equals` メソッドと `hashCode` メソッド**。それらは、オブジェクトのフィールド値に基づいて動作します。

   これは、`List` や `Set`、`Map` のようなコレクションでケースクラスを使いやすくします。また、オブジェクトを参照 ID の代わりに、それらの内容に基づいて比較できるようになります。

```tut:book
new Person("Noel", "Welsh").equals(new Person("Noel", "Welsh"))
new Person("Noel", "Welsh") == new Person("Noel", "Welsh")
```

4. **`copy` メソッド**。`copy` メソッドは、現在のオブジェクトと同じフィールド値を伴う新しいオブジェクトを生成します。

```tut:book
dave.copy()
```

   `copy` メソッドは、現在のオブジェクトの代わりに、クラスの**新しいオブジェクト**を生成して返すことに注意してください。

   実のところ、`copy` メソッドは、それぞれのコンストラクター引数と一致する任意の引数を受け取ります。引数が指定されれば、新しいオブジェクトは、既存のオブジェクトに存在する値の代わりにその値を使用します。これは、1つ以上のフィールド値を変更してオブジェクトをコピーするときに、キーワード引数と一緒に使用するのが理想的です。

```tut:book
dave.copy(firstName = "Dave2")
dave.copy(lastName = "Gurnell2")
```

<div class="callout callout-info">

#### 値と参照の等価性 {-}

Scala の `==` 演算子は Java のものとは異なります。それは、参照 ID を値として比較する代わりに `equals` に委譲します。

Scala は、Java の `==` と同じ振る舞いをする `eq` と呼ばれる演算子を持ちます。しかしながら、それをアプリケーションコードで使用することはめったにありません。

```tut:book
new Person("Noel", "Welsh") eq (new Person("Noel", "Welsh"))
dave eq dave
```
</div>

5. ケースクラスは `java.io.Serializable` と `scala.Product` という2つのトレイトを実装します。どちらも直接使用されることはありません。後者は、ケースクラスの名前とフィールド数を調べるためのメソッドを提供します。

### ケースクラスコンパニオンオブジェクトの機能

コンパニオンオブジェクトは、クラスのコンストラクターと同じ引数を持つ `apply` メソッドを含みます。Scala プログラマーは、`new` を省略できる簡潔さのために、コンストラクターより `apply` メソッドを好む傾向があり、コンストラクターが式の中で読み易くなるようにします。

```tut:book
Person("Dave", "Gurnell") == Person("Noel", "Welsh")
Person("Dave", "Gurnell") == Person("Dave", "Gurnell")
```

最後に、コンパニオンオブジェクトは、**パターンマッチング (pattern matching)** に使用する**抽出子パターン (extractor pattern)** を実装するコードも含みます。本章で後ほど見ていきます。

<div class="callout callout-info">
#### ケースクラス宣言文法 {-}

ケースクラスを宣言するための文法は、

```scala
case class Name(parameter: type, ...) {
  declarationOrExpression ...
}
```

です。ここで、

- `Name` はケースクラスの名前
- `parameter` はコンストラクター引数として与えられた名前（オプション）
- `type` はコンストラクター引数の型
- `declarationOrExpression` は宣言か式（オプション）

とします。
</div>

### ケースオブジェクト

最後の覚え書きです。コンストラクター引数なしでケースクラスを定義しているものをみつけたら、その代わりに**ケースオブジェクト (case object)** を定義できます。ケースオブジェクトは、単に通常のシングルトンオブジェクトのように定義されますが、もっと意味のある `toString` メソッドを持ち、`Product` と `Serializable` トレイトを継承しています。

```tut:book:silent
case object Citizen {
  def firstName = "John"
  def lastName  = "Doe"
  def name = firstName + " " + lastName
}
```

```tut:book
Citizen.toString
```

### 覚えておいてほしいこと

ケースクラスは **Scala データ型の真髄**です。それを使って、学んで、好きになってください。

ケースクラスを宣言するための文法は、`case` を付与すること以外はクラスを宣言するためのものと同じです。

```scala
case class Name(parameter: type, ...) {
  declarationOrExpression ...
}
```


ケースクラスは、タイピングの手間を省く、たくさんの自動生成されたメソッドと機能を持ちます。関連するメソッドを実装することによって、その動作を個別にオーバーライドできます。

Scala 2.10 以前は、0〜22のフィールドを含むケースクラスしか定義できませんでした。Scala 2.11 で、任意サイズのケースクラスを定義する能力を獲得しました。

### Exercises

#### Case Cats

Recall that a `Cat` has a `String` colour and food. Define a case class to represent a `Cat`.

<div class="solution">
Another simple finger exercise.

```tut:book:silent
case class Cat(colour: String, food: String)
```
</div>

#### Roger Ebert Said it Best...

> No good movie is too long and no bad movie is short enough.

The same can't always be said for code, but in this case we can get rid of a lot of boilerplate by converting `Director` and `Film` to case classes. Do this conversion and work out what code we can cut.

<div class="solution">

Case classes provide our `copy` methods and our `apply` methods and remove the need to write val` before each constructor argument. The final codebase looks like this:

```tut:book:silent
case class Director(firstName: String, lastName: String, yearOfBirth: Int) {
  def name: String =
    s"$firstName $lastName"
}

object Director {
  def older(director1: Director, director2: Director): Director =
    if (director1.yearOfBirth < director2.yearOfBirth) director1 else director2
}

case class Film(
  name: String,
  yearOfRelease: Int,
  imdbRating: Double,
  director: Director) {

  def directorsAge =
    yearOfRelease - director.yearOfBirth

  def isDirectedBy(director: Director) =
    this.director == director
}

object Film {
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

Not only is this code significantly shorter, it also provides us with `equals` methods, `toString` methods, and pattern matching functionality that will set us up for later exercises.
</div>

#### Case Class Counter

Reimplement `Counter` as a case class, using `copy` where appropriate. Additionally initialise `count` to a default value of `0`.

<div class="solution">
```tut:book:silent
case class Counter(count: Int = 0) {
  def dec = copy(count = count - 1)
  def inc = copy(count = count + 1)
}
```

This is almost a trick exercise---there are very few differences with the previous implementation However, notice the extra functionality we got for free:

```tut:book
Counter(0) // construct objects without `new`
Counter().inc // printout shows the value of `count`
Counter().inc.dec == Counter().dec.inc // semantic equality check
```
</div>

#### Application, Application, Application

What happens when we define a companion object for a case class? Let's see.

Take our `Person` class from the previous section and turn it into a case class (hint: the code is above). Make sure you still have the companion object with the alternate `apply` method as well.

<div class="solution">
Here's the code:

```tut:book:silent
case class Person(firstName: String, lastName: String) {
  def name = firstName + " " + lastName
}
```

```scala
object Person {
  def apply(name: String): Person = {
    val parts = name.split(" ")
    apply(parts(0), parts(1))
  }
}
```

Even though we are defining a companion object for `Person`, Scala's case class code generator is still working as expected---it adds the auto-generated companion methods to the object we have defined, which is why we need to place the class and companion in a single compilation unit.

This means we end up with a companion object with an overloaded `apply` method with two possible type signatures:

```scala
def apply(name: String): Person =
  // etc...

def apply(firstName: String, lastName: String): Person =
  // etc...
```
</div>
