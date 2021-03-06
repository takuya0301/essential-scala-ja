## リテラルオブジェクト

すでに Scala の基本型のいくつかを取り上げました。本節では、Scala におけるすべての**リテラル式**を扱うことによって、その知識に磨きをかける予定です。リテラル式は「それ自身」が象徴する固定値を表現します。下記はひとつの例です。

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

### Null 値

Null 値は Java と同じであるにもかかわらず、ほとんど頻繁には使用されません。また、Scala の `null` は独自の `Null` 型を持ちます。

```tut:book
null
```

<div class="callout callout-info">
#### Scala で Null 値を使用する {-}

`null` は Java コードで一般的ですが、Scala では非常に悪い習慣であると考えられています。

Java における `null` の主な用途は、プログラム実行の異なる時点において、値の有無という**任意 (optional)** 値を実装することです。しかしながら、`null` 値はコンパイラーによってチェックできないため、`NullPointerException` という形で実行時エラーが発生する可能性があります。

のちほど、コンパイラーによってチェックされる任意値を定義する手段を Scala が持つことを見ていきましょう。これは `null` を使用する必要性を取り除き、プログラムをより安全にしてくれます。
</div>

### Unit 値

Scala で `()` と書かれる Unit 値は、Java の `void` に相当します。Unit 値は、`println` を使用して標準出力に印字するような、何の面白みもない値に評価される式の結果です。コンソールは Unit 値を印字しませんが、式の型を尋ねることで、実際に何らかの式の結果であることがわかります。

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

Unit 値は Scala において重要な概念です。Scala の文法構造の多くは、型と値を持つ**式**です。有用な値を得られない式のためのプレイスホルダーが必要で、Unit 値はまさにそれを提供してくれます。

### 覚えておいてほしいこと

本節では、基本的なデータ型を評価する**リテラル**式を見てきました。これらの基本型は、等価なものがない `Unit` を除いて、ほとんど Java と同じです。

すべてのリテラル式は**型**を持ち、**値**に評価されることに注意してください。これは、より複雑な Scala の式にも当てはまります。

次節では、独自のオブジェクトリテラルを定義する方法を学びます。

### 演習

#### 文字通りただのリテラル

下記の Scala リテラルの値と型は何ですか？

```tut:book:silent
42

true

123L

42.0
```

<div class="solution">
`42` は `Int` です。`true` は `Boolean` です。`123L` は `Long` です。`42.0` は `Double` です。

この演習は、Scala コンソールや Scala ワークシートを使用した体験をするだけです。
</div>

#### 引用と誤引用

下記のリテラルの違いは何ですか？それぞれの型と値は？

```tut:book:silent
'a'

"a"
```

<div class="solution">
1つ目は `Char` リテラルであり、2つ目は `String` リテラルです。
</div>

#### 副作用についての余談

下記の式の違いは何ですか？それぞれの型と値は？

```tut:book:silent
"Hello world!"

println("Hello world!")
```

<div class="solution">
リテラル式 `"Hello world!"` は `String` の値として評価されます。式 `println("Hello world!")` は `Unit` として評価され、副作用としてコンソールに `"Hello world!"` を印字します。

これは、値を評価するプログラムと、値を副作用として印字するプログラムとの間の重要な区別です。前者はより大きな式で使用できますが、後者は使用できません。
</div>

#### 失敗から学ぶ

下記のリテラルの型と値は何ですか？REPL や Scala ワークシートに書いてみて、何が起こるか見てみましょう！

```scala
'Hello world!'
```

<div class="solution">
エラーメッセージを見ることになるはずです。開発環境のエラーメッセージを読んで慣れることに時間をかけてください。すぐに他にもたくさん見るようになるので！
</div>
