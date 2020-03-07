# 式・型・値

本章では Scala プログラムの基本的な構成要素である*式*・*型*・*値*について見ていきます。それらのコンセプトを理解することが Scala プログラムがどのように動くのかというメンタルモデルを形成するために必要です。

## 最初のプログラム

Scala コンソールか Scala ワークシートに `"Hello world!"` と入力し、コンソールでリターンキーを押下するかワークシートを保存してください。このようなインタラクションが見られるはずです。

```tut:book
"Hello world!"
```

このプログラムについてはいろいろ言えることがあります。それは特別に*リテラル式*もしくは略してリテラルと呼ばれる単一の式で構成されていることです。

Scala は私たちのプログラムを実行（評価）します。Scala コンソールや Scala ワークシートでプログラムを評価するとき、プログラムの*型*とプログラムを評価した*値*という2つの情報を受け取ります。この場合は、型が `String` で、値が `"Hello world!"` です。

プログラムが生成した出力値 "Hello world!" はプログラムと同じに見えますが、その2つの間には違いがあります。リテラル式は入力したプログラムテキストである一方、コンソールが表示したものはプログラムを評価した結果です。（リテラル (literal) は、評価されたものがその文字通り (literally) に見えるので名付けられました。）

わずかに複雑なプログラムを見てみましょう。

```tut:book
"Hello world!".toUpperCase
```

このプログラムは*メソッド呼び出し*を加えることによって最初の例を拡張したものです。Scala における評価は左から右へ進みます。この例では、最初にリテラル `"Hello world!"` が評価されます。つぎに、その評価された結果に対しメソッド `toUpperCase` が呼ばれます。このメソッドは文字列値を大文字に変換したものを新しい文字列として返します。これがコンソールによって表示された最終的な値です。

繰り返しになりますが、このプログラムの型は `String` で、この場合はプログラムが `"HELLO WORLD!"` に評価されます。

### Compile-time and Run-time

There are two distinct stages that a Scala program goes through: first it is *compiled*, and if it compiles successfully it can then be *run* or evaluated. We refer to the first stage as *compile-time* and the latter as *run-time*.

When using the Scala console our programs are evaluated as soon as they compile, which gives the appearance that there is only one stage. It is important to understand that compile- and run-time really are distinct, as it is this distinction that allows us to properly understand the difference between types and values.

Compilation is a process of checking that a program makes sense. There are two ways in which a program must "make sense":

1. It must be *syntactically correct*, meaning the parts of the program must be arranged according to the grammar of the language. An example English sentence that is not syntactically correct is "on cat mat sat the". An example syntactically incorrect Scala program is

```tut:book:fail
toUpperCase."Hello world!"
```

2. It must *type check*, meaning it must obey certain constraints on what a sensible program is. An example English sentence that is syntactically correct but fails to make sense is "the mat sat on the cat". A simple program that would fail to type check is trying to convert a number to uppercase.

```tut:book:fail
2.toUpperCase
```

The concept of upper and lowercase doesn't make sense for numbers, and the type system will catch this error.

If a program passes the checks at compile-time it may then be run. This is the process of the computer performing the instructions in the program.

Even though a program successfully compiles it can still fail at run-time. Dividing an integer by zero causes a run-time error in Scala.

```tut:book:fail
2 / 0
```

The type of integers, `Int`, allows division so the program type checks. At run-time the program fails because there is no `Int` that can represent the result of the division.


### Expressions, Types, and Values

So what exactly are expressions, types, and values?

Expressions are part of a program's text---what we type into a file, or the console or worksheet. They are the main components of a Scala program. We will see other components, namely *definitions* and *statements*, in due course. Expressions exist at compile-time.

The defining characteristic of an expression is that it evaluates to a value. A value is information stored in the computer's memory. It exists at run-time. For example, the expression `2` evaluates to a particular sequence of bits in a particular location in the computer's memory.

We compute with values. They are entities that our programs can pass around and manipulate. For example, to compute the minimum of two numbers we might write a program like

```tut:book
2.min(3)
```

Here we have two values, `2` and `3`, and we combine them into a larger program that evaluates to `2`.

In Scala all values are *objects*, which has a particular meaning we will see shortly.

Now let's turn to types. Types are restrictions on our programs that limit how we can manipulate objects. We have already seen two types, `String` and `Int`, and seen that we can perform different operations depending on the type.

At this stage, the most important point about types is that *expressions have types but values do not*. We cannot inspect an arbitrary piece of the computer's memory and divine how to interpret it without knowing the program that created it. For example, in Scala the `Int` and `Float` types are both represented by 32-bits of memory. There are no tags or other indications that a given 32-bits should be interpreted as an `Int` or a `Float`.

We can show that types exist at compile-time by asking the Scala console to tell us the type of an expression that causes a run-time error.

```scala
:type 2 / 0
// Int
```

```tut:book:fail
2 / 0
```

We see that the expression `2 / 0` has type `Int` even though this expression fails when we evaluate it.

Types, which exist at compile-time, restrict us to writing programs that give a consistent interpretation to values. We cannot claim that a particular 32-bits is at one point an `Int` and another a `Float`. When a program type checks, Scala guarantees that all values are used consistently and thus it does not need to record type information in a value's representation. This process of removing type information is called *type erasure*[^type-erasure].

[^type-erasure]: This is not entirely true. The Java Virtual Machine, the program that runs Scala code, distinguishes between two kinds of objects. Primitive types don't store any type information along with the value they represent. Object types do store type information. However this type information is not complete and there are occasions where it is lost. Blurring the distinction between compile- and run-time is thus dangerous. If we never rely on type information being around at run-time (and the patterns we will show you do not) we will never run into these issues.

Types necessarily do not contain all possible information about the values that conform to the type. If they did, type checking would be equivalent to running the program. We have already seen that the type system will not prevent us from dividing an `Int` by zero, which causes a run-time error.

An key part of designing Scala code is deciding which error cases we wish to rule out using the type system. We will see that we can express many useful constraints in the type system, improving the reliability of our programs. We could implement a division operator that used the type system to express the possibility of error, if we decided this was important enough in our program. Using the type system well is one of the main themes of this book.


### Take Home Points

We must build a mental model of Scala programs if we are to use Scala. Three fundamental components of this model are *expressions*, *types*, and *values*.

Expressions are the parts of a program that evaluate to a value. They are the major part of a Scala program.

Expressions have types, which express some restrictions on programs. During *compile-time* the types of our programs are checked. If they are inconsistent then compilation fails and we cannot evaluate, or run, our program.

Values exist in the computer's memory, and are what a running program manipulates. All values in Scala are *objects*, the meaning of which we will discuss soon.


### Exercises

#### Type and Value

Using the Scala console or worksheet, determine the type and value of the following expressions:

```tut:book:silent
1 + 2
```

<div class="solution">
Type is `Int` and value is `3`.
</div>

```tut:book:silent
"3".toInt
```

<div class="solution">
Type is `Int` and value is `3`.
</div>

```tut:book:fail:silent
"foo".toInt
```

<div class="solution">
Type is `Int`, but this one doesn't evaluate to a value---it raises an exception instead, and a raised exception is not a value. How can we tell this? We can't continue computing with the result of the expression. For example, we can't print it. Compare

```tut:book
println("foo")
```

and

```tut:book:fail
println("foo".toInt)
```

In the latter no printing occurs indicating the `println` is never evaluated.
</div>
