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

### コードを実行する

コードを実行し、それがコンパイルされていること（つまり、タイプミスをしていないこと）と、また、テストが失敗すること（つまり、何らかのテストが実行されていること）を確認してください。なお、メソッド宣言の後にテストを配置する必要があるかもしれません。

### 本体を書く

メソッドの本体を書く準備が整いました。本書を通じて、いくつかのテクニックを修得していきます。現時点では、2つのテクニックを見ていきましょう。

#### 結果型を考える

最初のテクニックは、結果型を見ることです。この場合は `Double` です。どうやって `Double` の値を生成するのでしょうか？リテラルを書くこともできますが、この場合は明らかに正しくありません。`Double` を生成する他の方法は、何らかのオブジェクトのメソッドを呼び出すことで、それは次のテクニックがもたらします。

#### 入力型を考える

次のテクニックは、メソッドの入力引数の型を見ることです。この場合は `Double` です。`Double` を生成する必要があることは確立していますが、入力から `Double` を生成するためにはどのようなメソッドを呼び出せばいいのでしょうか？そのようなメソッドはたくさんありますが、ここで呼び出すべき正しいメソッドとして `*` を選択するためにはドメイン知識を活用しなければなりません。

完全なメソッドは下記のように書くことができます。

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
