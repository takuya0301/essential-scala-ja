## メソッドの書き方

前節では、メソッドの文法について見てきました。本書の主な目的のひとつは、文法を超えて、Scala プログラムを構築するための体系的な方法を提供することです。本節はそのような問題を扱う最初の節です。本節では、体系的にメソッドを構築する方法を見ていきます。Scala の経験を積んでいくうちに、この方法のいくつかの段階を省略することができますが、本書の中ではこの方法に従うことを**強く**推奨します。

アドバイスを具体的にするために、前節の演習を例として使用していきましょう。

**`calc` と呼ばれるオブジェクトを定義します。それは、`square` という `Double` を引数として受け取る、あなたが予想するとおり入力を2乗 (square) するメソッドを伴います。そして、計算の一部として `square` メソッドの呼び出しを含む `cube` と呼ばれる、入力を3乗 (cube) するメソッドを追加してください。**

### 入力と出力を特定する

最初の段階は、入力となる引数がもしあれば、その型とメソッドの結果型を特定することです。

多くの場合、演習では型を教えてくれるので、説明文からそのまま読み取ることができます。上の例で、入力型は `Double` になっています。結果型もまた `Double` になることが推論できます。

### テストケースを準備する

型だけでは物語のすべてを語れません。`Double` から `Double` への関数はたくさんありますが、2乗を実行しているものは少数です。そのため、メソッドの期待される振る舞いを例証するいくつかのテストケースを準備しましょう。

外部依存を避けたいので、本書ではテストライブラリを使用しないことにします。Scala が提供する `assert` 関数を使用することで手軽なテストライブラリを実装できます。`square` の例については、下記のようなテストケースが考えられます。

```scala
assert(square(2.0) == 4.0)
assert(square(3.0) == 9.0)
assert(square(-2.0) == 4.0)
```

### 宣言を書く

型とテストケースが準備できたので、メソッド宣言を書くことができます。その本体はまだ書けないので、その場所に Scala の便利な機能である `???` を使用しておきます。

```tut:book:silent
def square(in: Double): Double =
  ???
```

この段階は、前の段階で収集した情報から機械的に与えられるべきです。

### Run the Code

Run the code and check it compiles (and thus we haven't made any typos) and also that our tests fail (and thus are testing something). You may need to place the tests after the method declaration.

### Write the Body

We're now ready to write the body of our method. We will develop a number of techniques for this throughout the course. For now, we're going to look at two techniques.

#### Consider the Result Type

The first technique is to look at the result type, in this case `Double`. How can we create `Double` values? We could write a literal, but that obviously won't be correct in this case. The other way we know to create a `Double` is to call a method on some object, which brings us to the next technique.

#### Consider the Input Type

Our next technique is to look at the type of input parameters to the method. In this case we have a `Double`. We have established we need to create a `Double`, so what methods can we call to create a `Double` from our input? There are many such methods, and here we have to use our domain knowledge to select `*` as the correct method to call.

We can now write our complete method as

```tut:book:silent
def square(in: Double): Double =
  in * in
```

### Run the Code, Again

Finally we should run the code again and check that the tests all pass in this case.

This is very simple example but practicing the process now will serve you well for the more complicated examples we will encounter later.

<div class="callout callout-info">
#### Process for Writing Methods {-}

We have a six-step process for writing methods in a systematic way.

1. Identify the type of the inputs and output of the method.
2. Write some test cases for the expected output of the method given example input. We can use the `assert` function to write down these cases.
3. Write the method declaration using `???` for the body like so:

```scala
def name(parameter: type, ...): resultType =
 ???
```
4. Run the code to check the test cases do in fact fail.
5. Write the body of the method. We currently have two techniques to apply here:
   - consider the result type and how we can create an instance of it; and
   - consider the input type and methods we can call to transform it to the result type.
6. Run the code again and check the test cases pass.
</div>
