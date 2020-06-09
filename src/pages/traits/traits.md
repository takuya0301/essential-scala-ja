## トレイト

オブジェクトを生成するためのテンプレートであるクラスと同じように、トレイトはクラスを生成するためのテンプレートです。トレイトは、2つ以上のクラスを同じものとみなし、同じ操作を実装するという表現を可能にします。言い換えれば、トレイトは、すべてのクラスが共有している `Any` 基底型の外側で、共通の基底型を共有する複数のクラスを表現できるということです。

<div class="callout callout-info">
#### トレイト対 Java インターフェイス {-}

トレイトは、Java 8 の**デフォルトメソッド (default method)** を伴う**インターフェイス (interface)** にとてもよく似ています。Java 8 を使用したことがなければ、インターフェイスと**抽象クラス (abstract class)** を掛け合わせたようなものがトレイトであると考えてください。
</div>


### トレイトの例

トレイトの例から始めましょう。Web サイトの訪問者をモデリングすることを想像してください。訪問者には2つのタイプがあり、ひとつはサイトに登録済みの訪問者で、もうひとつは匿名の訪問者です。2つのクラスでこれをモデル化します。

```tut:book:silent
import java.util.Date

case class Anonymous(id: String, createdAt: Date = new Date())

case class User(
  id: String,
  email: String,
  createdAt: Date = new Date()
)
```

これらのクラス定義で、匿名訪問者と登録済み訪問者は、ID と作成日を持つと言えます。しかし、私たちは登録済み訪問者のメールアドレスしかわかりません。

ここには明らかな重複があり、同じ定義を2回書かない方が望ましいです。しかし、もっと重要なのは、2種類の訪問者のために何らかの共通の型を作るということです。`AnyRef` や `Any` ではない、何らかの共通の型を持てれば、どんな種類の訪問者でも動作するメソッドを書くことができます。それは、下記のようにトレイトで実現できます。

```tut:book:silent
import java.util.Date

trait Visitor {
  def id: String      // 各ユーザーに付与される一意の ID
  def createdAt: Date // ユーザーがサイトに初めて訪れた日付

  // この訪問者が初めて訪れてからどのくらいになるのか？
  def age: Long = new Date().getTime - createdAt.getTime
}

case class Anonymous(
  id: String,
  createdAt: Date = new Date()
) extends Visitor

case class User(
  id: String,
  email: String,
  createdAt: Date = new Date()
) extends Visitor
```

2つの変更に注目してください。

- `Visitor` トレイトを定義しました
- `extends` キーワードを使用することによって `Visitor` トレイトの派生型として `Anonymous` と `User` を宣言しました

`Visitor` トレイトは、どんな派生型でも必ず実装しなければならないインターフェイスを表現するので、`String` 型の `id` と `Date` 型の `createdAt` を実装しなければなりません。また、`Visitor` のどんな派生型でも、自動的に `Visitor` で定義されている `age` メソッドを持ちます。

`Visitor` トレイトを定義することによって、下記のように訪問者のどんな派生型でも動作するメソッドを書くことができます。

```tut:book:silent
def older(v1: Visitor, v2: Visitor): Boolean =
  v1.createdAt.before(v2.createdAt)
```

```tut:book
older(Anonymous("1"), User("2", "test@example.com"))
```

こちらの `older` メソッドは、`Visitor` の派生型である `Anonymous` と `User` のいずれでも呼び出すことができます。

<div class="callout callout-info">
#### トレイト文法 {-}

トレイトを宣言するには、

```scala
trait TraitName {
  declarationOrExpression ...
}
```

と書きます。トレイトの派生型であるクラスを宣言するには、

```scala
class Name(...) extends TraitName {
  ...
}
```

と書きます。より一般的には、ケースクラスを使用しますが、その文法は同じです。

```scala
case class Name(...) extends TraitName {
 ...
}
```
</div>

### Traits Compared to Classes

Like a class, a trait is a named set of field and method definitions. However, it differs from a class in a few important ways:

 - *A trait cannot have a constructor*---we can't create objects directly from a trait. Instead we can use a trait to create a class, and then create objects from that class. We can base as many classes as we like on a trait.

 - Traits can define *abstract methods* that have names and type signatures but no implementation. We saw this in the `Visitor` trait. We must specify the implementation when we create a class that extends the trait, but until that point we're free to leave definitions abstract.

Let's return to the `Visitor` trait to further explore abstract definitions. Recall the definition of `Visitor` is

```tut:book:silent
import java.util.Date

trait Visitor {
  def id: String      // Unique id assigned to each user
  def createdAt: Date // Date this user first visited the site

  // How long has this visitor been around?
  def age: Long = new Date().getTime - createdAt.getTime
}
```

`Visitor` prescribes two abstract methods. That is, methods which do not have an implementation but must be implemented by extending classes. These are `id` and `createdAt`. It also defines a concrete method, `age`, that is defined in terms of one of the abstract methods.

`Visitor` is used as a building block for two classes: `Anonymous` and `User`. Each class `extends Visitor`, meaning it inherits all of its fields and methods:

```tut:book
val anon = Anonymous("anon1")
anon.createdAt
anon.age
```

`id` and `createdAt` are abstract so they must be defined in extending classes. Our classes implement them as `vals` rather than `defs`. This is legal in Scala, which sees `def` as a more general version of `val`[^uap]. It is good practice to never define `val`s in a trait, but rather to use `def`. A concrete implementation can then implement it using using a `def` or `val` as appropriate.

[^uap]: This is all part of the [uniform access principle][link-uap] we saw in the exercises for [Object Literals](object-literals.html).

### Take Home Points

Traits are a way of *abstracting over classes* that have similar properties, just like classes are a way of abstracting over objects.

Using a traits has two parts. Declaring the trait

```scala
trait TraitName {
  declarationOrExpression ...
}
```

and extending the trait from a class (usually a case class)

```scala
case class Name(...) extends TraitName {
  ...
}
```

### Exercises

#### Cats, and More Cats

Demand for Cat Simulator 1.0 is exploding! For v2 we're going to go beyond the domestic cat to model `Tiger`s, `Lion`s, and `Panther`s in addition to the `Cat`. Define a trait `Feline` and then define all the different species as subtypes of `Feline`. To make things interesting, define:

- on `Feline` a `colour` as before;
- on `Feline` a `String` `sound`, which for a cat is `"meow"` and is `"roar"` for all other felines;
- only `Cat` has a favourite food; and
- `Lion`s have an `Int` `maneSize`.

<div class="solution">
This is mostly a finger exercise to get you used to trait syntax but there are a few interesting things in the solution.

```tut:book:silent
trait Feline {
  def colour: String
  def sound: String
}

case class Lion(colour: String, maneSize: Int) extends Feline {
  val sound = "roar"
}

case class Tiger(colour: String) extends Feline {
  val sound = "roar"
}

case class Panther(colour: String) extends Feline {
  val sound = "roar"
}

case class Cat(colour: String, food: String) extends Feline {
  val sound = "meow"
}
```

Notice that `sound` is not defined as a constructor argument. Since it is a constant, it doesn't make sense to give users a chance to modify it. There is a lot of duplication in the definition of `sound`. We could define a default value in `Feline` like so

```tut:book:silent
trait Feline {
  def colour: String
  def sound: String = "roar"
}
```

This is generally a bad practice. If we define a default implementation it should be an implementation that is suitable for all subtypes.

Another alternative to define an intermediate type, perhaps called `BigCat` that defines sound as `"roar"`. This is a better solution.

```tut:book:silent
trait BigCat extends Feline {
  override val sound = "roar"
}
```

```scala
case class Tiger(...) extends BigCat
case class Lion(...) extends BigCat
case class Panther(...) extends BigCat
```
</div>

#### Shaping Up With Traits

Define a trait called `Shape` and give it three abstract methods:

 - `sides` returns the number of sides;
 - `perimeter` returns the total length of the sides;
 - `area` returns the area.

Implement `Shape` with three classes: `Circle`, `Rectangle`, and `Square`. In each case provide implementations of each of the three methods. Ensure that the main constructor parameters of each shape (e.g. the radius of the circle) are accessible as fields.

**Tip:** The value of &pi; is accessible as `math.Pi`.

<div class="solution">
```tut:book:silent
trait Shape {
  def sides: Int
  def perimeter: Double
  def area: Double
}

case class Circle(radius: Double) extends Shape {
  val sides = 1
  val perimeter = 2 * math.Pi * radius
  val area = math.Pi * radius * radius
}

case class Rectangle(
  width: Double,
  height: Double
) extends Shape {
  val sides = 4
  val perimeter = 2 * width + 2 * height
  val area = width * height
}

case class Square(size: Double) extends Shape {
  val sides = 4
  val perimeter = 4 * size
  val area = size * size
}
```
</div>

#### Shaping Up 2 (Da Streets) {#sec:traits:shaping-up-2}

The solution from the last exercise delivered three distinct types of shape. However, it doesn't model the relationships between the three correctly. A `Square` isn't just a `Shape`---it's also a type of `Rectangle` where the width and height are the same.

Refactor the solution to the last exercise so that `Square` and `Rectangle` are subtypes of a common type `Rectangular`.

**Tip:** A trait can extend another trait.

<div class="solution">
The new code looks like this:

```tut:book:silent
// trait Shape ...

// case class Circle ...

sealed trait Rectangular extends Shape {
  def width: Double
  def height: Double
  val sides = 4
  override val perimeter = 2*width + 2*height
  override val area = width*height
}

case class Square(size: Double) extends Rectangular {
  val width = size
  val height = size
}

case class Rectangle(
  val width: Double,
  val height: Double
) extends Rectangular
```

Ensure your trait is `sealed` so the compiler can
check the exhaustiveness of any code you write
that handles objects of type `Rectangular` or `Shape`.
</div>
