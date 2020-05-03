## クラス

クラスは、似たようなメソッドやフィールドを持つオブジェクトを生成するためのテンプレートです。Scala では、クラスもまた型を定義し、クラスから生成されたすべてのオブジェクトは同じ型を共有します。これによって、前章の演習「やぁ、人間」が持つ問題を克服することができます。

### クラスを定義する

これは、シンプルな `Person` クラスの宣言です。

```tut:book:silent
class Person {
  val firstName = "Noel"
  val lastName = "Welsh"
  def name = firstName + " " + lastName
}
```

オブジェクト宣言のように、クラス宣言は名前（この場合 `Person`）を束縛し、また式ではありません。しかしながら、オブジェクト名と違って、式の中でクラス名を使用することはできません。クラスは値ではなく、クラスは異なる**名前空間 (namespace)** に住んでいます。

```tut:book:fail
Person
```

`new` 演算子を使用して、新しい `Person` オブジェクトを生成できます。オブジェクトは値なので、通常の方法でそれらのメソッドやフィールドにアクセスできます。

```tut:book
val noel = new Person
noel.firstName
```

オブジェクトの型が `Person` であることに注意してください。印字された値には、`@xxxxxxxx` という形式のコードが含まれており、そのオブジェクトを特定する一意の識別子です。`new` を呼び出すたびに、同じ型の別オブジェクトが生成されます。

```tut:book
noel
val newNoel = new Person
val anotherNewNoel = new Person
```

これは、引数として任意の `Person` を受け取るメソッドを書けることを意味します。

```tut:book:silent
object alien {
  def greet(p: Person) =
    "Greetings, " + p.firstName + " " + p.lastName
}
```

```tut:book
alien.greet(noel)
alien.greet(newNoel)
```

<div class="callout callout-info">
#### Java ヒント {-}

Scala クラスはすべて `java.lang.Object` の派生クラスであり、ほとんどの場合、Scala と同様に Java からも使用できます。`Person` におけるデフォルト印字の振る舞いは、`java.lang.Object` に定義されている `toString` メソッドに由来しています。
</div>

### コンストラクター

現状、`Person` クラスは全然役に立ちません。新しいオブジェクトを好きなだけ生成することができますが、すべて同じ `firstName` と `lastName` を持つためです。それでは、それぞれの人に異なる名前を与えるにはどうすればいいのでしょうか？

その解決策は、**コンストラクター (constructor)** を導入することで、それは新しいオブジェクトを生成するときに引数を渡すことを可能にします。

```tut:book:silent
class Person(first: String, last: String) {
  val firstName = first
  val lastName = last
  def name = firstName + " " + lastName
}
```

```tut:book
val dave = new Person("Dave", "Gurnell")
dave.name
```

コンストラクター引数 `first` と `last` はクラスの本体でのみ使用できます。オブジェクトの外側からデータにアクセスするには、`val` や `def` を使用してフィールドやメソッドを宣言しなければなりません。

コンストラクター引数とフィールドはしばしば冗長です。幸運なことに、Scala は両方を一度に宣言する便利な略記法を提供してくれています。コンストラクター引数の前に `val` キーワードをつけることで、Scala はそれらのフィールドを自動的に定義してくれるようになります。

```tut:book:silent
class Person(val firstName: String, val lastName: String) {
  def name = firstName + " " + lastName
}
```

```tut:book
new Person("Dave", "Gurnell").firstName
```

`val` フィールドは**不変 (immutable)** で、一度初期化された後にそれらの値を変更することはできません。Scala は**可変 (mutable)** フィールドを定義するために `var` キーワードも提供しています。

Scala プログラマーは、不変かつ副作用のないコードを書くことを好む傾向があるので、置換モデルを使用してそれらを導出することができます。本書では、もっぱら不変な `val` フィールドに焦点を当てていきます。

<div class="callout callout-info">

#### クラス宣言文法 {-}

クラスを宣言するための文法は、

```scala
class Name(parameter: type, ...) {
  declarationOrExpression ...
}
```

か

```scala
class Name(val parameter: type, ...) {
  declarationOrExpression ...
}
```

です。ここで、

- `Name` はクラスの名前
- `parameter` はコンストラクター引数に与えられた名前（オプション）
- `type` はコンストラクター引数の型
- `declarationOrExpression` は宣言や式（オプション）

とします。
</div>

### デフォルト引数とキーワード引数

Scala のすべてのメソッドとコンストラクターは、**キーワード引数 (keyword parameter)** と**デフォルト引数値 (default parameter value)** に対応しています。

メソッドやコンストラクターを呼び出すときに、任意の順番で引数を指定するための**キーワードとして引数名を使用する**ことができます。

```tut:book
new Person(lastName = "Last", firstName = "First")
```

これは、下記のように定義された**デフォルト引数値**と組み合わせて使用するといっそう便利です。

```tut:book:silent
def greet(firstName: String = "Some", lastName: String = "Body") =
  "Greetings, " + firstName + " " + lastName + "!"
```

引数がデフォルト値を持つのであれば、メソッド呼び出しでその引数を省略できます。

```tut:book
greet("Busy")
```

デフォルト引数値とキーワードを組み合わせることで、前の引数を省略し、後の引数だけに値を渡すこともできます。

```tut:book
greet(lastName = "Dave")
```

<div class="callout callout-info">
#### キーワード引数 {-}

**キーワード引数は、引数の数や順番の変更に対して堅牢です。**例えば、`greet` メソッドに `title` 引数を追加すると、キーワードなしのメソッド呼び出しにおいて意味が変化してしまいますが、キーワードありの呼び出しでは同じままです。

```tut:book:silent
def greet(title: String = "Citizen", firstName: String = "Some", lastName: String = "Body") =
  "Greetings, " + title + " " + firstName + " " + lastName + "!"
```

```tut:book
greet("Busy") // これは正しくなくなりました
greet(firstName = "Busy") // これは正しいままです
```

これは、多数の引数を伴うメソッドやコンストラクターを作成するとき、特に便利です。
</div>

### Scala の型階層

プリミティブ型とオブジェクト型を区別する Java とは異なり、Scala ではすべてがオブジェクトです。その結果、`Int` や `Boolean` のような「プリミティブ」値の型は、クラスやトレイトと同じ型階層の一部を構成しています。


\makebox[\linewidth]{\includegraphics[width=0.8\textwidth]{src/pages/classes/scala-type-hierarchy.pdf}}

<div class="figure">
<div class="text-center">
<img src="src/pages/classes/scala-type-hierarchy.svg" alt="Scala 型階層" />
</div>
</div>

Scala は `Any` と呼ばれる最上位の基底型を持ち、その下に `AnyVal` と `AnyRef` という2つの型があります。`AnyVal` はすべての値型の基底型で、`AnyRef` はすべての参照型やクラスの基底型です。Scala や Java におけるすべてのクラスは `AnyRef` の派生型です[^value-classes]。

[^value-classes]: 実際には `AnyVal` の派生型を定義でき、それは[値クラス](http://docs.scala-lang.org/overviews/core/value-classes.html)として知られています。これらはいくつかの特殊な状況で有用なのですが、ここでは議論しません。

それらの型のいくつかは、単純に Java に存在する型の Scala における別名で、`Int` は `int`、`Boolean` は `boolean`、`AnyRef` は `java.lang.Object` です。

階層の**最下位 (bottom)** に2つの特別な型があります。`Nothing` は `throw` 式の型で、`Null` は `null` 値の型です。それらの特別な型は、他のすべての型の派生型で、コードにおける他の型を健全に保ちながらも、`throw` や `null` に型を割り当てることを補助します。下記のコードはこのことを説明しています。

```tut:book
def badness = throw new Exception("Error")
def otherbadness = null
val bar = if(true) 123 else badness
val baz = if(false) "it worked" else otherbadness
```

`badness` と `otherbadness` の型はそれぞれ `Nothing` と `Null` であるにも関わらず、`bar` と `baz` の型は実用的なままです。なぜならば、`Int` は `Int` と `Nothing` の最小共通基底型で、`String` は `String` と `Null` の最小共通基底型であるからです。

### 覚えておいてほしいこと

本節では、同じ**型**を持つ様々なオブジェクトの生成を可能にする**クラス**を定義する方法を学びました。このように、クラスを使うことで、似たような性質を持つ**オブジェクトを横断的に抽象化**することができます。

クラスにおいてもオブジェクトの性質は、**フィールド**と**メソッド**の形をとります。フィールドはオブジェクトに格納される事前に計算された値で、メソッドは呼び出すことができる計算です。

クラスを宣言するための文法は、

```scala
class Name(parameter: type, ...) {
  declarationOrExpression ...
}
```

です。

`new` キーワードを使用し、コンストラクターを呼び出すことによって、クラスからオブジェクトを生成します。

また、**キーワード引数**と**デフォルト引数**についても学びました。

最後に、Scala の型階層について、Java の型階層との重複や、特殊な型である `Any` や `AnyRef`、`AnyVal`、`Nothing`、`Null`、`Unit` を含むこと、そして Java のクラスと Scala のクラスは、どちらも型階層の同じサブツリーを占有するという事実を学びました。

### 演習

今ではクラスで楽しく遊ぶために十分な機構を手に入れました。

#### 猫、再び

以前の演習に登場した猫を思い出しましょう。

+--------------------------+------------------------+------------------+
| 名前 (Name)              | 色 (Colour)            | 食べ物 (Food)    |
+==========================+========================+==================+
| オズワルド (Oswald)      | 黒 (Black)             | ミルク (Milk)    |
+--------------------------+------------------------+------------------+
| ヘンダーソン (Henderson) | 茶トラ (Ginger)        | カリカリ (Chips) |
+--------------------------+------------------------+------------------+
| クエンティン (Quentin)   | トラ (Tabby and white) | カレー (Curry)   |
+--------------------------+------------------------+------------------+


`Cat` クラスを定義し、上の表の各猫についてオブジェクトを生成してください。

<div class="solution">
これはクラスを定義する文法に慣れるための指の運動です。

```tut:book:silent
class Cat(val colour: String, val food: String)

val oswald = new Cat("Black", "Milk")
val henderson = new Cat("Ginger", "Chips")
val quentin = new Cat("Tabby and white", "Curry")
```

</div>


#### Cats on the Prowl

Define an object `ChipShop` with a method `willServe`. This method should accept a `Cat` and return `true` if the cat's favourite food is chips, and false otherwise.

<div class="solution">
```tut:book:silent
object ChipShop {
  def willServe(cat: Cat): Boolean =
    if(cat.food == "Chips")
      true
    else
      false
}
```
</div>


#### Directorial Debut

Write two classes, `Director` and `Film`, with fields and methods as follows:

 - `Director` should contain:
    - a field `firstName` of type `String`
    - a field `lastName` of type `String`
    - a field `yearOfBirth` of type `Int`
    - a method called `name` that accepts no parameters
      and returns the full name

 - `Film` should contain:
    - a field `name` of type `String`
    - a field `yearOfRelease` of type `Int`
    - a field `imdbRating` of type `Double`
    - a field `director` of type `Director`
    - a method `directorsAge` that returns
      the age of the director at the time of release
    - a method `isDirectedBy` that accepts a `Director`
      as a parameter and returns a `Boolean`

```tut:book:invisible
case class Director(firstName: String, lastName: String, yearOfBirth: Int) {
  def name: String = firstName + " " + lastName
}
case class Film(name: String, yearOfRelease: Int, imdbRating: Double, director: Director) {
  def directorsAge: Int =
    yearOfRelease - director.yearOfBirth

  def isDirectedBy(d: Director): Boolean =
    director == d
}
```

Copy-and-paste the following demo data into your code and adjust your constructors so that the code works without modification:

```tut:book:silent
val eastwood          = new Director("Clint", "Eastwood", 1930)
val mcTiernan         = new Director("John", "McTiernan", 1951)
val nolan             = new Director("Christopher", "Nolan", 1970)
val someBody          = new Director("Just", "Some Body", 1990)

val memento           = new Film("Memento", 2000, 8.5, nolan)
val darkKnight        = new Film("Dark Knight", 2008, 9.0, nolan)
val inception         = new Film("Inception", 2010, 8.8, nolan)

val highPlainsDrifter = new Film("High Plains Drifter", 1973, 7.7, eastwood)
val outlawJoseyWales  = new Film("The Outlaw Josey Wales", 1976, 7.9, eastwood)
val unforgiven        = new Film("Unforgiven", 1992, 8.3, eastwood)
val granTorino        = new Film("Gran Torino", 2008, 8.2, eastwood)
val invictus          = new Film("Invictus", 2009, 7.4, eastwood)

val predator          = new Film("Predator", 1987, 7.9, mcTiernan)
val dieHard           = new Film("Die Hard", 1988, 8.3, mcTiernan)
val huntForRedOctober = new Film("The Hunt for Red October", 1990, 7.6, mcTiernan)
val thomasCrownAffair = new Film("The Thomas Crown Affair", 1999, 6.8, mcTiernan)
```

```tut:book
eastwood.yearOfBirth
dieHard.director.name
invictus.isDirectedBy(nolan)
```

Implement a method of `Film` called `copy`. This method should accept the same parameters as the constructor and create a new copy of the film. Give each parameter a default value so you can copy a film changing any subset of its values:


```tut:book:invisible
case class Director(firstName: String, lastName: String, yearOfBirth: Int) {
  def name: String = firstName + " " + lastName

  def copy(
    firstName: String = this.firstName,
    lastName: String = this.lastName,
    yearOfBirth: Int = this.yearOfBirth): Director =
    new Director(firstName, lastName, yearOfBirth)
}

case class Film(name: String, yearOfRelease: Int, imdbRating: Double, director: Director) {
  def directorsAge: Int =
    yearOfRelease - director.yearOfBirth

  def isDirectedBy(d: Director): Boolean =
    director == d

  def copy(
    name: String = this.name,
    yearOfRelease: Int = this.yearOfRelease,
    imdbRating: Double = this.imdbRating,
    director: Director = this.director): Film =
    new Film(name, yearOfRelease, imdbRating, director)
}

val eastwood          = new Director("Clint", "Eastwood", 1930)
val mcTiernan         = new Director("John", "McTiernan", 1951)
val nolan             = new Director("Christopher", "Nolan", 1970)
val someBody          = new Director("Just", "Some Body", 1990)

val memento           = new Film("Memento", 2000, 8.5, nolan)
val darkKnight        = new Film("Dark Knight", 2008, 9.0, nolan)
val inception         = new Film("Inception", 2010, 8.8, nolan)

val highPlainsDrifter = new Film("High Plains Drifter", 1973, 7.7, eastwood)
val outlawJoseyWales  = new Film("The Outlaw Josey Wales", 1976, 7.9, eastwood)
val unforgiven        = new Film("Unforgiven", 1992, 8.3, eastwood)
val granTorino        = new Film("Gran Torino", 2008, 8.2, eastwood)
val invictus          = new Film("Invictus", 2009, 7.4, eastwood)

val predator          = new Film("Predator", 1987, 7.9, mcTiernan)
val dieHard           = new Film("Die Hard", 1988, 8.3, mcTiernan)
val huntForRedOctober = new Film("The Hunt for Red October", 1990, 7.6, mcTiernan)
val thomasCrownAffair = new Film("The Thomas Crown Affair", 1999, 6.8, mcTiernan)
```

```tut:book
highPlainsDrifter.copy(name = "L'homme des hautes plaines")
thomasCrownAffair.copy(yearOfRelease = 1968,
  director = new Director("Norman", "Jewison", 1926))
inception.copy().copy().copy()
```

<div class="solution">
This exercise provides some hands on experience writing Scala classes, fields and methods. The model solution is as follows:

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
    yearOfBirth: Int = this.yearOfBirth): Director =
    new Director(firstName, lastName, yearOfBirth)
}

class Film(
  val name: String,
  val yearOfRelease: Int,
  val imdbRating: Double,
  val director: Director) {

  def directorsAge =
    yearOfRelease - director.yearOfBirth

  def isDirectedBy(director: Director) =
    this.director == director

  def copy(
    name: String = this.name,
    yearOfRelease: Int = this.yearOfRelease,
    imdbRating: Double = this.imdbRating,
    director: Director = this.director): Film =
    new Film(name, yearOfRelease, imdbRating, director)
}
```
</div>

#### A Simple Counter

Implement a `Counter` class. The constructor should take an `Int`. The methods `inc` and `dec` should increment and decrement the counter respectively returning a new `Counter`. Here's an example of the usage:

```tut:book:invisible
class Counter(val count: Int) {
  def dec = new Counter(count - 1)
  def inc = new Counter(count + 1)
}
```

```tut:book
new Counter(10).inc.dec.inc.inc.count
```

<div class="solution">
```tut:book:silent
class Counter(val count: Int) {
  def dec = new Counter(count - 1)
  def inc = new Counter(count + 1)
}
```

Aside from practicing with classes and objects, this exercise has a second goal---to think about why `inc` and `dec` return a new `Counter`, rather than updating the same counter directly.

Because `val` fields are immutable, we need to come up with some other way of propagating the new value of `count`. Methods that return new `Counter` objects give us a way of returning new state without the side-effects of assignment. They also permit *method chaining*, allowing us to write whole sequences of updates in a single expression

The use-case `new Counter(10).inc.dec.inc.inc.count` actually creates 5 instances of `Counter` before returning its final `Int` value. You may be concerned about the extra memory and CPU overhead for such a simple calculation, but don't be. Modern execution environments like the JVM render the extra overhead of this style of programming negligible in all but the most performance critical code.
</div>

#### Counting Faster

Augment the `Counter` from the previous exercise to allow the user can optionally pass an `Int` parameter to `inc` and `dec`. If the parameter is omitted it should default to `1`.

<div class="solution">
The simplest solution is this:

```tut:book:silent
class Counter(val count: Int) {
  def dec(amount: Int = 1) = new Counter(count - amount)
  def inc(amount: Int = 1) = new Counter(count + amount)
}
```

However, this adds parentheses to `inc` and `dec`. If we omit the parameter we now have to provide an empty pair of parentheses:

```tut:book:fail
new Counter(10).inc
```

We can work around this using *method overloading* to recreate our original parenthesis-free methods. Note that overloading methods requires us to specify the return types:

```tut:book:silent
class Counter(val count: Int) {
  def dec: Counter = dec()
  def inc: Counter = inc()
  def dec(amount: Int = 1): Counter = new Counter(count - amount)
  def inc(amount: Int = 1): Counter = new Counter(count + amount)
}
```

```tut:book
new Counter(10).inc.inc(10).count
```
</div>

#### Additional Counting

Here is a simple class called `Adder`.

```tut:book:silent
class Adder(amount: Int) {
  def add(in: Int) = in + amount
}
```

Extend `Counter` to add a method called `adjust`. This method should accept an `Adder` and return a new `Counter` with the result of applying the `Adder` to the `count`.

<div class="solution">
```tut:book:silent
class Counter(val count: Int) {
  def dec = new Counter(count - 1)
  def inc = new Counter(count + 1)
  def adjust(adder: Adder) =
    new Counter(adder.add(count))
}
```

This is an interesting pattern that will become more powerful as we learn more features of Scala. *We are using `Adders` to capture computations* and pass them to `Counter`. Remember from our earlier discussion that *methods are not expressions*---they cannot be stored in fields or passed around as data. However, *`Adders` are both objects and computations*.

Using objects as computations is a common paradigm in object oriented programming languages. Consider, for example, the classic `ActionListener` from Java's Swing:

```java
public class MyActionListener implements ActionListener {
  public void actionPerformed(ActionEvent evt) {
    // Do some computation
  }
}
```

The disadvantage of objects like `Adders` and `ActionListeners` is that they are limited to use in one particular circumstance. Scala includes a much more general concept called *functions* that allow us to represent any kind of computation as an object. We will be introduced to some of the concepts behind functions in this chapter.
</div>
