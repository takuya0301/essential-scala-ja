## オブジェクトとの相互作用


前節では Scala プログラムの基本的な構成要素である式・型・値を見てきました。また、*すべての値はオブジェクトである*ことも学びました。本節では、オブジェクトとオブジェクトに対してどのように相互作用するのかについて学んでいきます。

### オブジェクト

オブジェクトはデータとそのデータへの操作のグループです。例えば、`2` はオブジェクトです。データは整数の2で、データへの操作はなじみのある `+` や `-` などです。

オブジェクトのデータと操作について特別な専門用語があります。操作は*メソッド*として知られています。データは*フィールド*に保持されます。

### メソッド呼び出し

メソッド*呼び出し*によってオブジェクトを作用させます[^patterns]。メソッド呼び出しの例をいくつかすでに見てきました。例えば、`String` の大文字版をその `toUpperCase` メソッドを呼び出すことによって取得できることを見ました。

```tut:book
"hello".toUpperCase
```

一部のメソッドは*引数*を受け取り、それはメソッドがどのように動作するかを制御します。例えば、`take` メソッドは `String` から文字を取り出します。いくつの文字を取り出したいかを指定する引数を `take` に渡さなければなりません。

```tut:book
"abcdef".take(3)
"abcdef".take(2)
```

<div class="callout callout-info">
#### メソッド呼び出し文法 {-}

メソッド呼び出しのための文法は、

```scala
anExpression.methodName(param1, ...)
```

か

```scala
anExpression.methodName
```

である。ここで、

- `anExpression` はオブジェクトに評価される任意の式
- `methodName` はメソッドの名前
- `param1, ...` はメソッドの引数に評価される1つ以上の式

とする。
</div>

メソッド呼び出しは式なのでオブジェクトに評価されます。これは、より複雑なプログラムをつくるために、メソッド呼び出しを連鎖できるということを意味します。

```tut:book
"hello".toUpperCase.toLowerCase
```

メソッド呼び出しにおける様々な式はどの順番で評価されるのでしょうか？メソッド引数は、メソッドが呼び出される前に左から右に評価されます。下記の式では、

```tut:book
"Hello world!".take(2 + 3)
```

最初に式 `"Hello world!"` が、次に `2 + 3`（これは、最初に `2` を、次に `3` を評価する必要があります。）が、そして最後に `"Hello world!".take(5)` が評価されます。

### 演算子

Scala においてすべての値はオブジェクトであるため、`Int` や `Boolean` のようなプリミティブ型についてもメソッドを呼び出すことができます。これは `int` や `boolean` がオブジェクトではない Java と対照的です。

```tut:book
123.toShort // これは Scala で `Short` を定義する方法です
123.toByte // これは `Byte` を定義する方法です
```

しかし、`Int` がオブジェクトであれば、`+` や `-` のような基本的な算術演算子は何でしょうか？それらもまたメソッドでしょうか？そうです。Scala のメソッドは英数字の名前と同じようにシンボルの名前を持つことができます。

```tut:book
43 - 3 + 2
43.-(3).+(2)
```

（Scala 2.10 以前においては、`43.` を `Double` として解釈されることを防ぐために、`(43).-(3).+(2)` と書く必要があるので注意してください。）

<div class="callout callout-info">
#### 中置演算子記法 {-}

Scala において `a.b(c)` と書かれた任意の式は `a b c` と書ける。

`a b c d e` が等価であるのは `a.b(c).d(e)` で、`a.b(c, d, e)` ではないことに注意する。
</div>

シンボルの名前を持つか、英数字の名前を持つかにかかわらず、1つの引数を受け取るどんなメソッドでも *中置演算子記法* を使用できます。

```tut:book:silent
"the quick brown fox" split " "
// res: Array[String] = Array(the, quick, brown, fox)
```

中置記法は、いくつかの文法的速記のひとつで、冗長なメソッド呼び出しの代わりに、簡潔な演算子式を書くことを可能にします。*前置*・*後置*・*右結合*・*代入演算子*という記法もありますが、中置記法に比べると一般的ではありません。

中置演算子の結合は何の優先順位規則を支持すべきか？という質問はそれ自身をもたらします。Scala は、数学や論理の直観的知識にならい、メソッド名として使用する識別子から得られる [優先順位規則][link-precedence-rules] 一式を使用します。

```tut:book
2 * 3 + 4 * 5
(2 * 3) + (4 * 5)
2 * (3 + 4) * 5
```

### Take home points

All Scala values are objects. We *interact with objects by calling methods* on them. If you come from a Java background note we can call methods on `Int` or any other primitive value.

The syntax for a method call is

```scala
anExpression.methodName(parameter, ...)
```

or

```scala
anExpression methodName parameter
```

*Scala has very few operators - almost everything is a method call.* We use syntactic conventions like infix operator notation to keep our code simple and readable, but we can always fall back to standard method notation where it makes sense.

As we will see, Scala's focus on programming with expressions allows us to write much shorter code than we can in Java. It also allows us to reason about code in a very intuitive way using values and types.

### Exercises

#### Operator Style

Rewrite in operator-style

```tut:book
"foo".take(1)
```

<div class="solution">
```tut:book
"foo" take 1
```
</div>

Rewrite in method call style

```tut:book
1 + 2 + 3
```

<div class="solution">
```tut:book
1.+(2).+(3)
```
</div>

#### Substitution

What is the difference between the following expressions? What are the similarities?

```tut:book:silent
1 + 2 + 3

6
```

<div class="solution">
The expressions have the same result type and return value. However, they arrive at their results in different ways. The first computes its result through a series of additions, while the later is simply a literal.

As neither expression has any side-effects, they are interchangeable from a user's point of view. Anywhere you can write `1 + 2 + 3` you can also write `6`, and vice versa, without changing the meaning of any program. This is known as *substitution* and you may remember the principle from simplifying algebraic formulae at school.

As programmers we must develop a mental model of how our code operates. The *substitution model of evaluation* is a particuarly simple model that says anywhere we see an expression we may substitute its result. In the absence of side-effects, the substitution model always works[^side-effects]. If we know the types and values of each component of an expression, we know the type and value of the expression as a whole. In functional programming we aim to avoid side-effects for this reason: it makes our programs easier to understand.
</div>

[^side-effects]: What exactly is a side-effect? One workable definition is anything that causes substitution to yield an incorrect result. Does substitution *always* work, in the absence of side-effects? To truly have a correct model of Scala we must define the order in which we apply substitutions. There are a number of possible orders. (For example, we perform substitution left-to-right, or right-to-left? Do we substitute as soon as possible or delay until we need a value?) Most of the time order of substitution doesn't matter, but there are cases where it does. Scala always applies substitution from left-to-right and at the earliest possible time.

[^patterns]: パターンマッチングと呼ばれるオブジェクトを作用させる別の方法があります。パターンマッチングはのちほど紹介します。