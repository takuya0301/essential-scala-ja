## ケースクラス

**ケースクラス (case class)** は、クラスやコンパニオンオブジェクト、たくさんの実用的なデフォルト機能をまとめて定義してくれる非常に便利な略記法です。それは、やっかいごとを最小限にして、軽量なデータ保持クラスを生成するための理想的な方法になります。

ケースクラスは、クラス定義にキーワード `case` を前置するだけで生成されます。

```tut:book:silent
case class Person(firstName: String, lastName: String) {
  def name = firstName + " " + lastName
}
```

ケースクラスを定義するとき、いつでも Scala は自動的に**クラスとコンパニオンオブジェクト**を生成します。

```tut:book
val dave = new Person("Dave", "Gurnell") // クラスを持つ
Person // コンパニオンオブジェクトも
```

さらに、クラスとコンパニオンオブジェクトには、とても便利ないくつかの機能があらかじめ用意されています。

### ケースクラスの機能

1. **各コンストラクター引数のフィールド**。コンストラクター定義に `val` と書く必要はありませんし、明示的に書いても悪影響はありません。

```tut:book
dave.firstName
```

2. **デフォルト `toString` メソッド**。それは、クラスのコンストラクターに似た実用的な表現を印字します。もう `@` 記号と謎めいた16進数とはおさらばです。

```tut:book
dave
```

3. **実用的な `equals` メソッドと `hashCode` メソッド**。それらは、オブジェクトのフィールド値に基づいて動作します。

   これは、`List` や `Set`、`Map` のようなコレクションでケースクラスを使いやすくします。また、オブジェクトを参照 ID の代わりに、それらの内容に基づいて比較できるようになります。

```tut:book
new Person("Noel", "Welsh").equals(new Person("Noel", "Welsh"))
new Person("Noel", "Welsh") == new Person("Noel", "Welsh")
```

4. **`copy` メソッド**。`copy` メソッドは、現在のオブジェクトと同じフィールド値を伴う新しいオブジェクトを生成します。

```tut:book
dave.copy()
```

   `copy` メソッドは、現在のオブジェクトの代わりに、クラスの**新しいオブジェクト**を生成して返すことに注意してください。

   実のところ、`copy` メソッドは、それぞれのコンストラクター引数と一致する任意の引数を受け取ります。引数が指定されれば、新しいオブジェクトは、既存のオブジェクトに存在する値の代わりにその値を使用します。これは、1つ以上のフィールド値を変更してオブジェクトをコピーするときに、キーワード引数と一緒に使用するのが理想的です。

```tut:book
dave.copy(firstName = "Dave2")
dave.copy(lastName = "Gurnell2")
```

<div class="callout callout-info">

#### 値と参照の等価性 {-}

Scala の `==` 演算子は Java のものとは異なります。それは、参照 ID を値として比較する代わりに `equals` に委譲します。

Scala は、Java の `==` と同じ振る舞いをする `eq` と呼ばれる演算子を持ちます。しかしながら、それをアプリケーションコードで使用することはめったにありません。

```tut:book
new Person("Noel", "Welsh") eq (new Person("Noel", "Welsh"))
dave eq dave
```
</div>

5. ケースクラスは `java.io.Serializable` と `scala.Product` という2つのトレイトを実装します。どちらも直接使用されることはありません。後者は、ケースクラスの名前とフィールド数を調べるためのメソッドを提供します。

### ケースクラスコンパニオンオブジェクトの機能

コンパニオンオブジェクトは、クラスのコンストラクターと同じ引数を持つ `apply` メソッドを含みます。Scala プログラマーは、`new` を省略できる簡潔さのために、コンストラクターより `apply` メソッドを好む傾向があり、コンストラクターが式の中で読み易くなるようにします。

```tut:book
Person("Dave", "Gurnell") == Person("Noel", "Welsh")
Person("Dave", "Gurnell") == Person("Dave", "Gurnell")
```

最後に、コンパニオンオブジェクトは、**パターンマッチング (pattern matching)** に使用する**抽出子パターン (extractor pattern)** を実装するコードも含みます。本章で後ほど見ていきます。

<div class="callout callout-info">
#### ケースクラス宣言文法 {-}

ケースクラスを宣言するための文法は、

```scala
case class Name(parameter: type, ...) {
  declarationOrExpression ...
}
```

です。ここで、

- `Name` はケースクラスの名前
- `parameter` はコンストラクター引数として与えられた名前（オプション）
- `type` はコンストラクター引数の型
- `declarationOrExpression` は宣言か式（オプション）

とします。
</div>

### ケースオブジェクト

最後の覚え書きです。コンストラクター引数なしでケースクラスを定義しているものをみつけたら、その代わりに**ケースオブジェクト (case object)** を定義できます。ケースオブジェクトは、単に通常のシングルトンオブジェクトのように定義されますが、もっと意味のある `toString` メソッドを持ち、`Product` と `Serializable` トレイトを継承しています。

```tut:book:silent
case object Citizen {
  def firstName = "John"
  def lastName  = "Doe"
  def name = firstName + " " + lastName
}
```

```tut:book
Citizen.toString
```

### 覚えておいてほしいこと

ケースクラスは **Scala データ型の真髄**です。それを使って、学んで、好きになってください。

ケースクラスを宣言するための文法は、`case` を付与すること以外はクラスを宣言するためのものと同じです。

```scala
case class Name(parameter: type, ...) {
  declarationOrExpression ...
}
```


ケースクラスは、タイピングの手間を省く、たくさんの自動生成されたメソッドと機能を持ちます。関連するメソッドを実装することによって、その動作を個別にオーバーライドできます。

Scala 2.10 以前は、0〜22のフィールドを含むケースクラスしか定義できませんでした。Scala 2.11 で、任意サイズのケースクラスを定義する能力を獲得しました。

### 演習

#### ケースキャット

`Cat` が色とエサを `String` で持っていることを思い出してください。`Cat` を表現するケースクラスを定義しましょう。

<div class="solution">
もうひとつの簡単な指の運動です。

```tut:book:silent
case class Cat(colour: String, food: String)
```
</div>

#### ロジャー・イーバートは言う……

> 良くない映画は長すぎるが、悪くない映画は十分に短い。

コードについては必ずしも同じことを言えませんが、この場合、`Director` と `Film` をケースクラスに変換することによって、多くの定型文を取り除くことができます。この変換を実行し、どのコードを削除できるか試してみましょう。

<div class="solution">

ケースクラスは `copy` メソッドと `apply` メソッドを提供し、各コンストラクター引数の前にある `val` を記述する必要性を取り除きます。最終的なコードベースは下記のようになります。

```tut:book:silent
case class Director(firstName: String, lastName: String, yearOfBirth: Int) {
  def name: String =
    s"$firstName $lastName"
}

object Director {
  def older(director1: Director, director2: Director): Director =
    if (director1.yearOfBirth < director2.yearOfBirth) director1 else director2
}

case class Film(
  name: String,
  yearOfRelease: Int,
  imdbRating: Double,
  director: Director) {

  def directorsAge =
    yearOfRelease - director.yearOfBirth

  def isDirectedBy(director: Director) =
    this.director == director
}

object Film {
  def newer(film1: Film, film2: Film): Film =
    if (film1.yearOfRelease < film2.yearOfRelease) film1 else film2

  def highestRating(film1: Film, film2: Film): Double = {
    val rating1 = film1.imdbRating
    val rating2 = film2.imdbRating
    if (rating1 > rating2) rating1 else rating2
  }

  def oldestDirectorAtTheTime(film1: Film, film2: Film): Director =
    if (film1.directorsAge > film2.directorsAge) film1.director else film2.director
}
```

このコードはかなり短くなっただけでなく、`equals` メソッドや `toString` メソッド、後ほどの演習の準備にもなるパターンマッチング機能を提供します。
</div>

#### ケースクラスカウンター

ケースクラスとして `Counter` を再実装し、適切なところで `copy` を使用してください。さらに、`0` をデフォルト値として `count` を初期化しましょう。

<div class="solution">
```tut:book:silent
case class Counter(count: Int = 0) {
  def dec = copy(count = count - 1)
  def inc = copy(count = count + 1)
}
```

これは、ほとんど引っ掛け問題です。前回の実装と、ほんの少しの違いしかありません。しかしながら、無料で得られている追加の機能に注目してください。

```tut:book
Counter(0) // `new` なしでオブジェクトを構築
Counter().inc // 印字で `count` の値を表示
Counter().inc.dec == Counter().dec.inc // 意味のある等価性を検証
```
</div>

#### 適用、適用、適用

ケースクラスにコンパニオンオブジェクトを定義すると何が起こるでしょうか？見てみましょう。

前節の `Person` クラスを題材に、ケースクラスへの変換を試してみましょう。（ヒント：コードは上にあります。）代替 `apply` メソッドを伴うコンパニオンオブジェクトを依然として持っていることを確認してください。

<div class="solution">
こちらがコードです。

```tut:book:silent
case class Person(firstName: String, lastName: String) {
  def name = firstName + " " + lastName
}
```

```scala
object Person {
  def apply(name: String): Person = {
    val parts = name.split(" ")
    apply(parts(0), parts(1))
  }
}
```

`Person` のためにコンパニオンオブジェクトを定義しているにも関わらず、依然として Scala のケースクラスのコードは期待どおりに動作します。自動生成されたメソッドを定義したコンパニオンオブジェクトに追加するので、ひとつのコンパイル単位にクラスとコンパニオンオブジェクトを配置する必要があります。

これは、最終的に `apply` メソッドのオーバーロードによって、2つの型シグネチャをコンパニオンオブジェクトが持つこと意味します。

```scala
def apply(name: String): Person =
  // etc...

def apply(firstName: String, lastName: String): Person =
  // etc...
```
</div>
