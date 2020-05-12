## 関数としてのオブジェクト

前節、最後の演習で `Adder` と呼ばれるクラスを定義しました。

```tut:book:silent
class Adder(amount: Int) {
  def add(in: Int): Int = in + amount
}
```

議論の中で、計算を表現するオブジェクトとしての `Adder` を説明しました。それは、値として渡すことのできるメソッドを得られたようなものでした。

計算のように振る舞うオブジェクトは強力な概念で、Scala にはそれを生成するための言語機能が完全に備わっています。それらのオブジェクトは**関数 (function)** と呼ばれ、**関数型プログラミング (functional programming)** の基礎を成します。

### apply メソッド

ここでは、関数型プログラミングをサポートする Scala の一機能**関数適用文法 (function application syntax)** を見ていきましょう。

Scala では、慣例によって、`apply` と呼ばれるメソッドを持つオブジェクトを関数のように「呼び出す」ことができます。`apply` と名付けられたメソッドによって、呼び出し文法 `foo.apply(args)` が `foo(args)` になるという特別な略記法が与えられます。

例えば、`Adder` の `add` メソッドを `apply` に改名してみましょう。

```tut:book:silent
class Adder(amount: Int) {
  def apply(in: Int): Int = in + amount
}
```

```tut:book
val add3 = new Adder(3)
add3.apply(2)
add3(4) // add3.apply(4) の略記法
```

この簡単なトリックによって、オブジェクトは文法的に関数らしく「見える」ようになります。オブジェクトを、変数に代入したり、引数として受け渡したり、メソッドではできなかったことがたくさんできます。

<div class="callout callout-info">

#### 関数適用文法 {-}

メソッド呼び出し `object.apply(parameter, ...)` は `object(parameter, ...)` と書くこともできる。

</div>

### 覚えておいてほしいこと

本節では、オブジェクトを関数であるかのように「呼び出す」ための**関数適用文法**を見ました。

関数適用文法は、`apply` メソッドが定義されたどんなオブジェクトでも利用可能です。

関数適用文法によって、計算のように振る舞う第一級値（訳注：[第一級関数](https://ja.wikipedia.org/wiki/%E7%AC%AC%E4%B8%80%E7%B4%9A%E9%96%A2%E6%95%B0)）を持てるようになりました。メソッドと違って、オブジェクトはデータとして受け渡すことができます。これで、Scala における真の関数型プログラミングに一歩近付きました。

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
