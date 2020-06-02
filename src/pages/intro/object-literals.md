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

下記の表は、3匹の猫の名前 (name)・色 (colour)・好きなエサ (food) を示しています。それぞれの猫についてのオブジェクトを定義してください。（経験豊富なプログラマーの方へ：クラスについてはまだ説明していません。）

+--------------------------+------------------------+------------------+
| 名前 (Name)              | 色 (Colour)            | エサ (Food)    |
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


#### スクウェア・ダンス！

`calc` と呼ばれるオブジェクトを定義します。それは、`square` という `Double` を引数として受け取る、あなたが予想するとおり入力を2乗 (square) するメソッドを伴います。そして、計算の一部として **`square` メソッドの呼び出し** を含む `cube` と呼ばれる、入力を3乗 (cube) するメソッドを追加してください。

<div class="solution">
これが解答です。`cube(x)` は `square(x)` を呼び出し、その値を `x` によってもう一度乗算します。各メソッドの結果型はコンパイラーによって `Double` として推論されます。

```tut:book:silent
object calc {
  def square(x: Double) = x * x
  def cube(x: Double) = x * square(x)
}
```
</div>

#### 精密なスクウェア・ダンス！

前の演習から `calc` をコピー＆ペーストして、`Int` と同様に `Double` でも動作するよう一般化された `calc2` を作成してください。Java の経験を持っていれば、これはかなり簡単にできるはずです。そうでなければ、下記の解答を読んでください。

<div class="solution">
Java のように Scala は特に `Int` と `Double` の間でうまく一般化できるわけではありません。しかしながら、引数の型ごとに `square` メソッドと `cube` メソッドを定義することで**オーバーロード (overload)** することができます。

```tut:book:silent
object calc2 {
  def square(value: Double) = value * value
  def cube(value: Double) = value * square(value)

  def square(value: Int) = value * value
  def cube(value: Int) = value * square(value)
}
```

「オーバーロードされた」メソッドとは、異なる引数型で複数回定義したメソッドのことです。オーバーロードされたメソッド型を呼び出すたびに、Scala は引数の型を見ることによって、どの変種が必要かを自動的に判断します。

```tut:book
calc2.square(1.0) // `square` の `Double` 版を呼び出す
calc2.square(1)   // `square` の `Int` 版を呼び出す
```

Scala コンパイラーは、低い精度から高い精度が必要な場合に、数値型間の自動変換を挿入できます。例えば、`calc.square(2)` と書けば、コンパイラーは `calc.square` の唯一のバージョンが `Double` を受け取ると判断し、本当のところは `calc.square(2.toDouble)` という意図であると自動的に推論します。

高い精度から低い精度への逆方向の変換は、丸め誤差につながる可能性があるため、自動的には処理されません。例えば、下記のコードは、`x` が `Int` であり、その本体の式が `Double` であるため、コンパイルされません！（実際に試してみてください。）

```tut:book:fail
val x: Int = calc.square(2) // コンパイルエラー
```

これは、`Double` の `toInt` メソッドを手動で使用することで回避できます。

```tut:book
val x: Int = calc.square(2).toInt // toInt は切り捨て
```

<div class="callout callout-warning">
#### 文字列連結の危険性 {-}

Java と似た振る舞いを維持するために、Scala もまた必要に応じて任意のオブジェクトを自動的に `String` に変換します。これは、`println("a" + 1)` のようなものを簡単に書けるようにするためで、Scala は `println("a" + 1.toString)` に自動的に書き換えます。

文字列連結と数値加算が同じ `+` メソッドを共有しているという事実が、予期せぬバグを引き起こすことがあるので注意しましょう！
</div>
</div>

#### 評価の順番

コンソールで入力したとき、下記プログラムの出力は何で、最終的な式の型と値は何になるでしょうか？各フィールドやメソッドの型や依存関係、評価の振る舞いをよく考えてみてください。

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
これが解答です。

```tut:book
argh.c + argh.b + argh.a
```

評価の完全な順番は下記のとおりです。

```
- プログラムの最後にあるメインの合計を計算するには、
  - `argh` を読み込み、
    - `argh` のすべてのフィールドを計算するので、
      - `b` を計算すると、
        - `"b"` を印字し、
        - `a + 2` を評価するので、
          - `a` を呼び出し、
            - `"a"` を印字し、
            - `1` を返却し、
          - `1 + 2` を返却し、
        - `b` に値 `3` を格納し、
  - `argh.c` を呼び出し、
    - `"c"` を印字し、
    - `a` を評価し、
      - `"a"` を印字し、
      - `1` を返却するのですが、それは破棄され、
    - `b + "c"` を評価し、
      - `b` から値 `3` を取得し、
        - 値 `"c"` を取得し、
        - `+` を評価し、それが実際のところ文字列の連結を参照していると判断し、
          `3` を `"3"` に変換し、
        - 文字列 `"3c"` を返却し、
  - `argh.b` を呼び出し,
    - `b` から値 `3` を取得し、
  - 最初の `+` を評価し、それが実際のところ文字列の連結を参照していると判断し、
    `"3c3"` を生成し、
  - `argh.a` を呼び出し、
    - `"a"` を印字し、
    - `1` を返却し、
  - 最初の `+` を評価し、それが実際のところ文字列の連結を参照していると判断し、
    `"3c31"` を生成する
```

ふぅ、こんな簡単なコードにしてはたくさんありますね。
</div>

#### やぁ、人間

`person` というオブジェクトを定義します。それは、`firstName` と `lastName` というフィールドを含みます。`alien` という2番目のオブジェクトを定義します。それは、引数として `person` を受け取り、その `firstName` を使用して挨拶する `greet` というメソッドを含みます。

`greet` メソッドの型は何でしょうか？このメソッドを使用して他のオブジェクトに挨拶することはできますか？

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

`greet` の引数 `p` の型 `person.type` に注目してください。これは、先ほど言及した**シングルトン型 (singleton type)** のひとつです。この場合は `person` オブジェクトに固有の型なので、他のオブジェクトで `greet` を使用することはできません。これは、Scala のすべての整数で共有されている `Int` のような型とは大きく異なります。

これでは、Scala でプログラムを書く能力に大きな制限を課してしまいます。組み込み型か独自に作成した単一のオブジェクトで動作するメソッドしか書けないのです。有用なプログラムを構築するために、**独自の型を定義**し、それぞれの型で多様な値を生成する機能が必要です。クラス (`class`) を使用してこれを実現できるのですが、それは次節で扱います。
</div>

#### メソッドの真価

メソッドは値ですか？式ですか？なぜそうなるのでしょうか？

<div class="solution">
まず、メソッドと式の等価性を扱いましょう。すでに知っているように、式は値を生成するプログラムの断片です。何かが式であるかどうかの簡単なテストは、それをフィールドに代入できるかどうかを確認することです。

```tut:book:silent
object calculator {
  def square(x: Int) = x * x
}
```

```tut:book:fail
val someField = calculator.square
```

このエラーメッセージをまだ完全に理解はできません（「部分適用関数 (partially applied function)」については後ほど学びます。）が、`square` が**式ではない**ことを示しています。しかしながら、`square` の**呼び出し**は値を生成**する**のです。

```tut:book
val someField = calculator.square(2)
```

引数を持たないメソッドは異なる振る舞いをしているように見えます。しかしながら、これは文法のトリックです。

```tut:book:silent
object clock {
  def time = System.currentTimeMillis
}
```

```tut:book
val now = clock.time
```

`clock.time` を値として `now` に割り当てているように見えるにも関わらず、実際にそれは **`clock.time` の呼び出しによって返された値**を代入しています。これは、もう一度メソッドを呼び出すことで実証できます。

```tut:book
val aBitLaterThanNow = clock.time
```

上で見たように、Scala では「フィールドへの参照」と「引数のないメソッド呼び出し」は同じように見えます。これは、他のコードに影響を与えることなく、フィールドの実装をメソッドに置き換えることができる（逆も然り）ように設計されています。それは、**[統一アクセス原理 (uniform access principle)][link-uap]** と呼ばれるプログラミング言語の機能です。

つまり、要約すると、**メソッド呼び出しは式である**が**メソッド自体は式ではない**ということです。Scala は**関数 (function)** と呼ばれる概念を持っており、それはメソッドのように呼び出せるオブジェクトです。オブジェクトが値であることをすでに知っているように、関数は値であり、データとして取り扱うことができます。お気付きのとおり、関数は**関数型プログラミング (functional programming)** の重要な部分であり、Scala の大きな強みのひとつです。関数と関数型プログラミングについては少しずつ学んでいきましょう。
</div>
