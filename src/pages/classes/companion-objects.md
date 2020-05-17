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

#### Extended Body of Work

Write companion objects for `Director` and `Film` as follows:

 - the `Director` companion object should contain:
    - an `apply` method that accepts the same parameters as the constructor of the class
      and returns a new `Director`;
    - a method `older` that accepts two `Directors` and returns the oldest of the two.

 - the `Film` companion object should contain:
    - an `apply` method that accepts the same parameters as the constructor of the class
      and returns a new `Film`;
    - a method `highestRating` that accepts two `Films` and returns the highest
      `imdbRating` of the two;
    - a method `oldestDirectorAtTheTime` that accepts two `Films` and returns the `Director`
      who was oldest at the respective time of filming.

<div class="solution">

This exercise is inteded to provide more practice writing code. The model solution, including the class definitions from the previous section, is now:

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

#### Type or Value?

The similarity in naming of classes and companion objects tends to cause confusion for new Scala developers. When reading a block of code it is important to know which parts refer to a class or *type* and which parts refer to a singleton object or *value*.

This is the inspiration for the new hit quiz, *Type or Value?*, which we will be piloting below. In each case identify whether the word `Film` refers to the type or value:

```scala
val prestige: Film = bestFilmByChristopherNolan()
```

<div class="solution">
**Type!**---this code is defining a value `prestige` of type `Film`.
</div>

```scala
new Film("Last Action Hero", 1993, mcTiernan)
```

<div class="solution">
*Type!*---this is a reference to the *constructor* of `Film`. The constructor is part of the *class* `Film`, which is a *type*.
</div>

```scala
Film("Last Action Hero", 1993, mcTiernan)
```

<div class="solution">
*Value!*---this is shorthand for:

```scala
Film.apply("Last Action Hero", 1993, mcTiernan)
```

`apply` is a method defined on the *singleton object* (or value) `Film`.
</div>

```scala
Film.newer(highPlainsDrifter, thomasCrownAffair)
```

<div class="solution">
*Value!*---`newer` is another method defined on the *singleton object* `Film`.
</div>

Finally a tough one...

```scala
Film.type
```

<div class="solution">
*Value!*---This is tricky! You'd be forgiven for getting this one wrong.

`Film.type` refers to the type of the singleton object `Film`, so in this case `Film` is a reference to a value. However, the whole fragment of code is a type.
</div>
