## リテラルオブジェクト

すでに Scala の基本型のいくつかを取り上げました。本節では、Scala におけるすべての*リテラル式*を扱うことによって、その知識に磨きをかける予定です。リテラル式は「それ自身」が象徴する固定値を表現します。下記はひとつの例です。

```tut:book
42
```

この REPL での相互作用は、リテラル `42` が `Int` `42` に評価されることを示しています。

リテラルとそれが評価された値を混同してはいけません。リテラル式はプログラムを実行する前のプログラムテキストにおける表現で、値はプログラムを実行した後のコンピューターのメモリにおける表現です。

これまでにプログラミングの経験、とくに Java の経験があれば、Scala におけるリテラルに見覚えがあるはずです。

### 数値

数値は Java にある同じ型を共有しており、32ビット整数の `Int`、64ビット浮動小数点数の `Double`、32ビット浮動小数点数の `Float`、そして64ビット整数の `Long` があります。 

```tut:book
42
42.0
42.0f
42L
```

Scala は16ビット整数の `Short` と8ビットの `Byte` を持ちますが、それらを生成するためのリテラル文法はありません。その代わりに、`toShort` や `toByte` と呼ばれるメソッドを使用することで生成できます。

### 真偽値

真偽値は Java とちょうど同じで、`true` か `false` です。

```tut:book
true
false
```

### 文字値

`Chars` は16ビット Unicode 値で、シングルクォートで囲まれた単一の文字として書かれます。

```tut:book
'a'
```

<div class="callout callout-info">
#### Scala 対 Java の型階層 {-}

最初の文字が大文字で書かれていますが、Scala の `Int` や `Double`、`Float`、`Long`、`Short`、`Byte`、`Boolean`、`Char` は、Java の `int` や `double`、`float`、`long`、`short`、`byte`、`boolean`、`char` とちょうど同じものを参照します。

Scala において、それらすべての型はメソッドとフィールドを伴うオブジェクトのように振る舞います。しかしながら、一度コードがコンパイルされたら、Scala の `Int` は Java の `int` とちょうど同じです。これが2つの言語間の相互運用を簡単にしてくれるのです。
</div>

### 文字列値

文字列値はちょうど Java の文字列で、同じ方法で書かれます。

```tut:book
"this is a string"
"the\nusual\tescape characters apply"
```

### Null

Null is the same as Java, though not used nearly as often. Scala's `null` also has its own type: `Null`.

```tut:book
null
```

<div class="callout callout-info">
#### Using Nulls in Scala {-}

Although `null`s are common in Java code, they are considered very bad practice in Scala.

The main use of `null` in Java is to implement *optional* values that have some or no value at different points of a program's execution. However, `null` values cannot be checked by the compiler, leading to possible runtime errors in the form of `NullPointerExceptions`.

Later we will see that Scala has the means to define optional values that *are* checked by the compiler. This removes the necessity of using `null`, making our programs much safer.
</div>

### Unit

Unit, written `()`, is the Scala equivalent of Java's `void`. Unit is the result of expressions that evaluate to no interesting value, such as printing to standard output using `println`. The console doesn't print unit but we can ask for the type of an expression to see that unit is in fact the result of some expressions.

```tut:book
()
```

```scala
:type ()
// Unit
```

```tut:book
println("something")
```

```scala
:type println("something")
// Unit
```

Unit is an important concept in Scala. Many of Scala's syntactic constructs are *expressions* that have types and values. We need a placeholder for expressions that don't yield a useful value, and unit provides just that.

### Take home points

In this section we have seen *literal* expressions, which evaluate to basic data types. These basics types are mostly identical to Java, except for `Unit` which has no equivalent.

We note that every literal expression has a *type*, and evaluates to a *value*---something which is also true for more complex Scala expressions.

In the next section we will learn how to define our own object literals.

### Exercises

#### Literally Just Literals

What are the values and types of the following Scala literals?

```tut:book:silent
42

true

123L

42.0
```

<div class="solution">
`42` is an `Int`. `true` is a `Boolean`. `123L` is a `Long`. `42.0` is a `Double`.

This exercise just gives you some experience using the Scala console or Worksheet.
</div>

#### Quotes and Misquotes

What is the difference between the following literals? What is the type and value of each?

```tut:book:silent
'a'

"a"
```

<div class="solution">
The first is a literal `Char` and the second is a literal `String`.
</div>

#### An Aside on Side-Effects

What is the difference between the following expressions? What is the type and value of each?

```tut:book:silent
"Hello world!"

println("Hello world!")
```

<div class="solution">
The literal expression `"Hello world!"` evaluates to a `String` value. The expression `println("Hello world!")` evaluates to `Unit` and, as a side-effect, prints `"Hello world!"` on the console.

This an important distinction between a program that evaluates to a value and a program that prints a value as a side-effect. The former can be used in a larger expression but the latter cannot.
</div>

#### Learning By Mistakes

What is the type and value of the following literal? Try writing it on the REPL or in a Scala worksheet and see what happens!

```scala
'Hello world!'
```

<div class="solution">
You should see an error message. Take the time to read and get used to the error messages in your development environment---you'll see plenty more of them soon!
</div>
