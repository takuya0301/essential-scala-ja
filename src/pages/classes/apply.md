## 関数としてのオブジェクト

前節、最後の演習で `Adder` と呼ばれるクラスを定義しました。

```tut:book:silent
class Adder(amount: Int) {
  def add(in: Int): Int = in + amount
}
```

議論の中で、計算を表現するオブジェクトとしての `Adder` を説明しました。それは、値として渡すことのできるメソッドを得られたようなものでした。

計算のように振る舞うオブジェクトは強力な概念で、Scala にはそれを生成するための言語機能が完全に備わっています。それらのオブジェクトは**関数 (function)** と呼ばれ、**関数型プログラミング (functional programming)** の基礎を成します。

### The apply method

For now we are going to look at just one of Scala's features supporting functional programming---*function application syntax*.

In Scala, by convention, an object can be "called" like a function if it has a method called `apply`. Naming a method `apply` affords us a special shortened call syntax: `foo.apply(args)` becomes `foo(args)`.

For example, let's rename the `add` method in `Adder` to `apply`:

```tut:book:silent
class Adder(amount: Int) {
  def apply(in: Int): Int = in + amount
}
```

```tut:book
val add3 = new Adder(3)
add3.apply(2)
add3(4) // shorthand for add3.apply(4)
```

With this one simple trick, objects can "look" syntactically like functions. There are lots of things that we can do with objects that we can't do with methods, including assign them to variables and pass them around as arguments.

<div class="callout callout-info">

#### Function Application Syntax {-}

The method call `object.apply(parameter, ...)` can also be written as `object(parameter, ...)`

</div>

### Take home points

In this section we looked at *function application syntax*, which lets us "call" an object as if it is a function.

Function application syntax is available for any object defining an `apply` method.

With function application syntax, we now have first class values that behave like computations. Unlike methods, objects can be passed around as data. This takes us one step closer towards true functional programming in Scala.

### Exercises

#### When is a Function not a Function?

We'll get a chance to write some code at the end of the next section. For now we should think about an important theoretical question:

How close does function application syntax get us to creating truly reusable objects to do computations for us? What are we missing?

<div class="solution">
The main thing we're missing is *types*, which are the way we properly abstract across values.

At the moment we can define a class called `Adder` to capture the idea of adding to a number, but that code isn't properly portable across codebases---other developers need to know about our specific class to use it.

We could define a library of common function types with names like `Handler`, `Callback`, `Adder`, `BinaryAdder`, and so on, but this quickly becomes impractical.

Later on we will see how Scala copes with this problem by defining a generic set of function types that we can use in a wide variety of situations.
</div>
