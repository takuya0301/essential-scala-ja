## オブジェクトリテラル

ここまでは、`Int` や `String` のような組み込み型のオブジェクトを作成し、それらを式に組み合わせる方法を見てきました。本節では、**オブジェクトリテラル (object literals)** を使用して独自デザインのオブジェクトを作成する方法を見ていきます。

オブジェクトリテラルを書くとき、式とは別のプログラムの一種である**宣言 (declaration)** を使います。宣言は値を評価しません。その代わりに名前と値を関連付けます。この名前は他のコードで値を参照するために使用できます。

下記のように空のオブジェクトを宣言できます。

```tut:book:silent
object Test {}
```

これは値に評価されず式ではありません。むしろ、名前 `Test` を空のオブジェクト値に結び付けます。

一度 `Test` という名前に結び付ければ式の中で使用することができ、それは宣言したオブジェクトに評価されます。もっとも単純な式はそれ自身の名前だけで、それ自体が値として評価されます。

```tut:book
Test
```

この式は `123` や `"abc"` のようなリテラルを書くことと同じです。オブジェクトの型は `Test.type` として報告されることに注意してください。これは、これまで見てきた型とは異なり、オブジェクトのためだけに作成された新しい型で、**シングルトン型 (singleton type)** と呼ばれます。この型における他の値を作成することはできません。

空のオブジェクトはあまり便利ではありません。オブジェクト宣言の本体である中括弧の間には式を入れることができます。しかし、メソッドやフィールド、さらなるオブジェクトを宣言するような宣言を入れるのが一般的です。

<div class="callout callout-info">
#### オブジェクト宣言文法 {-}

オブジェクト宣言のための文法は、

```scala
object name {
  declarationOrExpression ...
}
```

です。ここで、

- `name` はオブジェクトの名前
- `declarationOrExpression` は宣言か式（オプション）

とします。
</div>

それではメソッドとフィールドをどのように宣言するか見ていきましょう。

### メソッド

メソッドによってオブジェクトを作用させるので、メソッドを伴うオブジェクトを生成してみましょう。

```tut:book:silent
object Test2 {
  def name: String = "Probably the best object ever"
}
```

ここでは `name` と呼ばれるメソッドを生成しました。これをいつもの方法で呼び出すことができます。

```tut:book
Test2.name
```

下記はより複雑なメソッドを伴うオブジェクトです。

```tut:book:silent
object Test3 {
  def hello(name: String) =
    "Hello " + name
}
```

```tut:book
Test3.hello("Noel")
```

<div class="callout callout-info">
#### メソッド宣言文法 {-}

メソッドを宣言するための文法は、

```scala
def name(parameter: type, ...): resultType =
  bodyExpression
```

か

```scala
def name: resultType =
  bodyExpression
```

です。ここで、

- `name` はメソッドの名前
- `parameter` はメソッドに与えられる引数の名前（オプション）
- `type` はメソッド引数の型
- `resultType` はメソッドの結果型（オプション）
- `bodyExpression` はメソッドを呼び出すことで評価される式

とします。メソッド引数はオプションですが、メソッドが引数を持つのであれば、それらの型は与えられなければなりません。結果型はオプションであるにもかかわらず、それを定義することが、機械的に検証されたドキュメントとしての役割を果たすので良い習慣とされます。

**引数 (argument)** という用語は**パラメーター (parameter)** と言い換えることができます。（訳注：本書ではどちらの用語も**引数**という訳語に統一しています。）
</div>

<div class="callout callout-info">
#### 返却は暗黙的 {-}

メソッドの返却値は本体を評価することによって決定されます。Java でするように `return` を書く必要はありません。
</div>


### フィールド

オブジェクトは**フィールド (field)** と呼ばれる他のオブジェクトを含めることもできます。`def` によく似た `val` や `var` というキーワードを使用して導入します。

```tut:book:silent
object Test4 {
  val name = "Noel"
  def hello(other: String): String =
    name + " says hi to " + other
}
```

```tut:book
Test4.hello("Dave")
```

<div class="callout callout-info">
#### フィールド宣言文法 {-}

フィールドを宣言するための文法は、

```scala
val name: type = valueExpression
```

か

```scala
var name: type = valueExpression
```

です。ここで、

- `name` はフィールドの名前
- `type` はフィールドの型（オプション）
- `valueExpression` は `name` に束縛されるオブジェクトに評価される式

とします。
</div>

`val` を使用することで**不変 (immutable)** フィールドを定義でき、これは名前に束縛された値の変更ができないことを意味します。`var` は**可変 (mutable)** フィールドで、束縛された値の変更ができます。

**常に `var` より `val` を選びましょう。** 置換を維持するため、Scala プログラマーは可能な限り不変フィールドを使用することを選びます。アプリケーションコードで時折可変フィールドを生成することは間違いありませんが、本書の大部分では `var` を使用しないようにしており、普段の Scala プログラミングにおいてもそれに倣いましょう。

### メソッド対フィールド

同じ動作をするように見える引数のないメソッドを持つことができるにも関わらず、なぜフィールドが必要なのか不思議に思うかもしれません。その違いはわずかで、フィールドは値に名前を与えますが、一方のメソッドは値を算出する計算に名前を与えます。

その違いを明らかにするオブジェクトがこちらです。

```tut:book:silent
object Test7 {
   val simpleField = {
     println("Evaluating simpleField")
     42
   }
   def noParameterMethod = {
     println("Evaluating noParameterMethod")
     42
   }
}
```

ここでは、コンソールに何かを印字するために `println` 式を、式をグループにするためにブロック式（`{` と `}` によって囲まれた式）を使用しています。なお、ブロック式については、次節でより詳しく見ていきます。

オブジェクトを定義したとコンソールに表示されているのに、いずれの `println` 文も実行されていないことに注意してください。これは**遅延読み込み (lazy loading)** と呼ばれる Scala と Java の特性によるものです。

オブジェクトやクラス（後述）は、他のコードによって参照されるまで読み込まれません。これが、単純な `"Hello world!"` アプリを実行するために、Scala が標準ライブラリ全体をメモリに読み込むことを防いでいます。

式の中で `Test7` を参照することで、Scala にオブジェクトの本体を強制的に評価させてみましょう。

```tut:book
Test7
```

オブジェクトが最初に読み込まれるとき、Scala はその定義を実行し、各フィールドの値を計算します。その結果、コードの副作用として `"Evaluating simpleField"` が印字されます。

**フィールドにおける本体の式は一度だけ実行されます。** その後、オブジェクトにその最終的な値が格納されます。下記で `println` 出力が欠けていることからわかるように、その式は二度と評価されることはありません。

```tut:book
Test7.simpleField
Test7.simpleField
```

一方、下記で `println` 出力が繰り返されていることからわかるように、メソッドの本体はメソッドを呼び出すたびに評価されます。

```tut:book
Test7.noParameterMethod
Test7.noParameterMethod
```

### 覚えておいてほしいこと

本節では、独自のオブジェクトを作成し、メソッドとフィールドを与え、式の中で参照しました。

文法としてオブジェクトの宣言、

```scala
object name {
  declarationOrExpression ...
}
```

メソッドの宣言、

```scala
def name(parameter: type, ...): resultType = bodyExpression
```

そしてフィールドの宣言、

```scala
val name = valueExpression
var name = valueExpression
```

を見てきました。これらすべては**宣言**で、名前と値を束縛します。宣言は式とは異なります。宣言は値を評価しませんし型も持ちません。

また、メソッドとフィールドの違いを見てきました。フィールドはオブジェクトの中に格納された値を参照し、一方のメソッドは値を算出する計算を参照します。

### 演習

#### キャット・オ・マティック

下記の表は、3匹の猫の名前 (name)・色 (colour)・好きな食べ物 (food) を示しています。それぞれの猫についてのオブジェクトを定義してください。（経験豊富なプログラマーの方へ：クラスについてはまだ説明していません。）

+--------------------------+------------------------+------------------+
| 名前 (Name)              | 色 (Colour)            | 食べ物 (Food)    |
+==========================+========================+==================+
| オズワルド (Oswald)      | 黒 (Black)             | ミルク (Milk)    |
+--------------------------+------------------------+------------------+
| ヘンダーソン (Henderson) | 茶トラ (Ginger)        | カリカリ (Chips) |
+--------------------------+------------------------+------------------+
| クエンティン (Quentin)   | トラ (Tabby and white) | カレー (Curry)   |
+--------------------------+------------------------+------------------+

<div class="solution">

これはオブジェクトを定義する文法に慣れるための指の運動にすぎません。下記コードのような解答が得られているはずです。

```tut:book:silent
object Oswald {
  val colour: String = "Black"
  val food: String = "Milk"
}

object Henderson {
  val colour: String = "Ginger"
  val food: String = "Chips"
}

object Quentin {
  val colour: String = "Tabby and white"
  val food: String = "Curry"
}
```

</div>


#### Square Dance!

Define an object called `calc` with a method `square` that accepts a `Double` as an argument and... you guessed it... squares its input. Add a method called `cube` that cubes its input *calling `square`* as part of its result calculation.

<div class="solution">
Here is the solution. `cube(x)` calls `square(x)` and multiplies its value by `x` one more time. The return type of each method is inferred by the compiler as `Double`.

```tut:book:silent
object calc {
  def square(x: Double) = x * x
  def cube(x: Double) = x * square(x)
}
```
</div>

#### Precise Square Dance!

Copy and paste `calc` from the previous exercise to create a `calc2` that is generalized to work with `Ints` as well as `Doubles`. If you have Java experience, this should be fairly straightforward. If not, read the solution below.

<div class="solution">
Like Java, Scala can't generalize particularly well across `Ints` and `Doubles`. However, it will allow us to *"overload"* the `square` and `cube` methods by defining them for each type of parameter.

```tut:book:silent
object calc2 {
  def square(value: Double) = value * value
  def cube(value: Double) = value * square(value)

  def square(value: Int) = value * value
  def cube(value: Int) = value * square(value)
}
```

"Overloaded" methods are ones we have defined several times for different argument types. Whenever we call an overloaded method type, Scala automatically determines which variant we need by looking at the type of the argument.

```tut:book
calc2.square(1.0) // calls the `Double` version of `square`
calc2.square(1)   // calls the `Int` version `square`
```

The Scala compiler is able to insert automatic conversions between numeric types wherever you have a lower precision and require a higher precision. For example, if you write `calc.square(2)`, the compiler determines that the only version of `calc.square` takes a `Double` and automatically infers that you really mean `calc.square(2.toDouble)`.

Conversions in the opposite direction, from high precision to low precision, are not handled automatically because they can lead to rounding errors. For example, the code below will not compile because `x` is an `Int` and its body expression is a `Double` (try it and see)!

```tut:book:fail
val x: Int = calc.square(2) // compile error
```

You can manually use the `toInt` method of `Double` to work around this:

```tut:book
val x: Int = calc.square(2).toInt // toInt rounds down
```

<div class="callout callout-warning">
#### The Dangers of String Concatenation {-}

To maintain similar behaviour to Java, Scala also automatically converts any object to a `String` where required. This is to make it easy to write things like `println("a" + 1)`, which Scala automatically rewrites as `println("a" + 1.toString)`.

The fact that string concatenation and numeric addition share the same `+` method can sometimes cause unexpected bugs, so watch out!
</div>
</div>

#### Order of evaluation

When entered on the console, what does the following program output, and what is the type and value of the final expression? Think carefully about the types, dependencies, and evaluation behaviour of each field and method.

```tut:book:silent
object argh {
  def a = {
    println("a")
    1
  }

  val b = {
    println("b")
    a + 2
  }

  def c = {
    println("c")
    a
    b + "c"
  }
}
```

```scala
argh.c + argh.b + argh.a
```

<div class="solution">
Here is the solution:

```tut:book
argh.c + argh.b + argh.a
```

The full sequence of evaluation is as follows:

```
- We calculate the main sum at the end of the program, which...
  - Loads `argh`, which...
    - Calculates all the fields in `argh`, which...
      - Calculates `b`, which...
        - Prints `"b"`
        - Evaluates `a + 2`, which...
          - Calls `a`, which...
            - Prints `"a"`
            - Returns `1`
          - Returns `1 + 2`
        - Stores the value `3` in `b`
  - Calls `argh.c`, which...
    - Prints `"c"`
    - Evaluates `a`
      - Prints `"a"`
      - Returns `1` - Which we discard
    - Evaluates `b + "c"`, which...
      - Retrieves the value `3` from `b`
        - Retrieves the value `"c"`
        - Evaluates the `+`, determining that it actually refers to string
          concatenation and converting `3` to `"3"`
        - Returns the `String` `"3c"`
  - Calls `argh.b`, which...
    - Retrieves the value `3` from `b`
  - Evaluates the first `+`, determining that it actually refers to string
    concatentation, and yielding `"3c3"`
  - Calls `argh.a`, which...
    - Prints `"a"`
    - Returns `1`
  - Evaluates the first `+`, determining that it actually refers to string
    concatentation, and yielding `"3c31"`
```

Whew! That's a lot for such a simple piece of code.
</div>

#### Greetings, human

Define an object called `person` that contains fields called `firstName` and `lastName`. Define a second object called `alien` containing a method called `greet` that takes your person as a parameter and returns a greeting using their `firstName`.

What is the type of the `greet` method? Can we use this method to greet other objects?

<div class="solution">
```tut:book:silent
object person {
  val firstName = "Dave"
  val lastName = "Gurnell"
}

object alien {
  def greet(p: person.type) =
    "Greetings, " + p.firstName + " " + p.lastName
}
```

```tut:book
alien.greet(person)
```

Notice the type on the `p` parameter of `greet`: `person.type`. This is one of the *singleton types* we were referring to earlier. In this case it is specific to the object `person`, which prevents us using `greet` on any other object. This is very different from a type such as `Int` that is shared by all Scala integers.

This imposes a significant limitation on our ability to write programs in Scala. We can only write methods that work with built-in types or single objects of our own creation. In order to build useful programs we need the ability to *define our own types* and create multiple values of each. We can do this using `classes`, which we will cover in the next section.
</div>

#### The Value of Methods

Are methods values? Are they expressions? Why might this be the case?

<div class="solution">
First let's deal with the equivalence between methods and expressions. As we know, expressions are program fragments that produce values. A simple test of whether something is an expression is to see if we can assign it to a field.

```tut:book:silent
object calculator {
  def square(x: Int) = x * x
}
```

```tut:book:fail
val someField = calculator.square
```

Although we don't understand this error message fully yet (we shall learn about "partially applied functions" later), it does show us that `square` *is not an expression*. However, a *call* to `square` *does* yield a value:

```tut:book
val someField = calculator.square(2)
```

A method with no arguments looks like it behaves differently. However, this is a trick of the syntax.

```tut:book:silent
object clock {
  def time = System.currentTimeMillis
}
```

```tut:book
val now = clock.time
```

Although it looks like `now` is being assigned `clock.time` as a value, it is actually being assigned the *value returned by calling `clock.time`*. We can demonstrate this by calling the method again:

```tut:book
val aBitLaterThanNow = clock.time
```

As we saw above, references to fields and calls to argumentless methods look identical in Scala. This is by design, to allow us to swap the implementation of a field for a method (and vice versa) without affecting other code. It is a programming language feature called the *[uniform access principle][link-uap]*.

So, in summary, *calls to methods are expressions* but *methods themselves are not expressions*. In addition to methods, Scala also has a concept called *functions*, which are objects that can be invoked like methods. As we know objects are values, so functions are also values and can be treated as data. As you may have guessed, functions are a critical part of *functional programming*, which is one of Scala's major strengths. We will learn about functions and functional programming in a bit.
</div>
