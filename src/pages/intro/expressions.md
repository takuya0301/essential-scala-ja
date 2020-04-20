## 複合式

これで Scala の基本的な入門編はほぼ終了しました。本節では、より複雑なプログラムで必要になるであろう、**条件式 (conditional)** と **ブロック (block)** という2種類の特別な式について見ていきます。

### 条件式

条件式は、ある条件にもとづいて評価する式を選択することを可能にします。例えば、2つの数値のうちどちらが小さいかにもとづいて文字列を選択することができます。

```tut:book
if(1 < 2) "Yes" else "No"
```

<div class="callout callout-info">
#### 条件式は式である {-}

Scala における `if` の記述は Java と同じ構文を持ちます。重要な違いのひとつは、**Scala の条件式は式である**ということで、それは型と返却値を持ちます。
</div>

選択されない式は評価されません。これは、副作用を伴う式を使用すると明らかです。

```tut:book
if(1 < 2) println("Yes") else println("No")
```

`No` がコンソールに出力されないので、`println("No")` という式は評価されていないことがわかります。

<div class="callout callout-info">
#### 条件式文法 {-}

条件式のための文法は、

```scala
if(condition)
  trueExpression
else
  falseExpression
```

です。ここで、

- `condition` は `Boolean` 型の式
- `trueExpression` は `condition` が `true` に評価されるときに評価される式
- `falseExpression` は `condition` が `false` に評価されるときに評価される式

とします。
</div>


### ブロック

ブロックは、計算をまとめて並べることができる式のことです。それは、セミコロンもしくは改行によって区切られた部分式を含む中括弧のペアとして書かれます。

```tut:book:fail
{ 1; 2; 3 }
```

ご覧のように、このコードを実行すると、コンソールはいくつかの警告を発生させ、`Int` 値 `3` を返します。

ブロックは、中括弧によって囲まれた一連の式や宣言のことです。ブロックもまた式であり、各部分式を順番に実行し、最後の式の値を返します。

`1` と `2` の値を捨ててしまうなら、なぜそれらを実行するのでしょうか？これはいい質問で、上記で Scala コンパイラーが警告を発生させた理由にもなります。

ブロックを使用するひとつの理由は、最終的な値を計算する前に副作用を生成するコードを使用したいからです。

```tut:book
{
  println("This is a side-effect")
  println("This is a side-effect as well")
  3
}
```

また、下記のように中間結果に名前をつけたいときにブロックを使用することもできます。

```tut:book:silent
def name: String = {
  val title = "Professor"
  val name = "Funkenstein"
  title + " " + name
}
```

```tut:book
name
```

<div class="callout callout-info">

#### ブロック式文法 {-}

ブロック式の文法は、

```scala
{
   declarationOrExpression ...
   expression
}
```

です。ここで、

- `declarationOrExpression` は宣言か式（オプション）
- `expression` はブロック式の型と値を決定する式

とします。
</div>

### Take home points

Conditional expressions allow us to choose an expression to evaluate based on a `Boolean` condition. The syntax is

```scala
if(condition)
  trueExpression
else
  falseExpression
```

A conditional, being an expression, has a type and evaluates to an object.


A block allows us to sequence expressions and declarations. It is commonly used when we want to sequence expressions with side-effects, or name intermediate results in a computation. The syntax is

```scala
{
   declarationOrExpression ...
   expression
}
```

The type and value of a block is that of the last expression in the block.


### Exercises

#### A Classic Rivalry

What is the type and value of the following conditional?

```tut:book:silent
if(1 > 2) "alien" else "predator"
```

<div class="solution">
It's a `String` with value `"predator"`. Predators are clearly best:

```tut:book
if(1 > 2) "alien" else "predator"
```

The type is determined by the upper bound of the types in the *then* and *else* expressions. In this case both expressions are `Strings` so the result is also a `String`.

The value is determined at runtime. `2` is greater than `1` so the conditional evaluates to the value of the *else* expression.
</div>

#### A Less Well Known Rivalry

What about this conditional?

```tut:book:silent
if(1 > 2) "alien" else 2001
```

<div class="solution">
It's a value of type `Any` with value `2001`:

```tut:book
if(1 > 2) "alien" else 2001
```

This is similar to the previous exercise---the difference is the type of the result. We saw earlier that the type is the *upper bound* of the positive and negative arms of the expression. `"alien"` and `2001` are completely different types - their closest common ancestor is `Any`, which is the grand supertype of all Scala types.

This is an important observation: types are determined at compile time, before the program is run. The compiler doesn't know which of `1` and `2` is greater before running the program, so it can only make a best guess at the type of the result of the conditional. `Any` is as close as it can get in this program, whereas in the previous exercise it can get all the way down to `String`.

We'll learn more about `Any` in the following sections. Java programmers shouldn't confuse it with `Object` because it subsumes value types like `Int` and `Boolean` as well.
</div>

#### An if Without an else

What about this conditional?

```tut:book:silent
if(false) "hello"
```

<div class="solution">
The result type and value are `Any` and `()` respectively:

```tut:book
if(false) "hello"
```

All code being equal, conditionals without `else` expressions only evaluate to a value half of the time. Scala works around this by returning the `Unit` value if the `else` branch should be evaluated. We would usually only use these expressions for their side-effects.
</div>
