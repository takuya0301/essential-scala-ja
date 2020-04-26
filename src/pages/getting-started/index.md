# はじめに

本書では、Scala コードの短い例を使って学んでいきます。それには2つの推奨される方法があります。（訳注：訳書において Scala IDE を使用する方法は非推奨です。理由は「Scala IDE を準備する」の節に記載しています。）

 1. **Scala コンソール (Scala console)** を使用する（コマンドラインが好きな人にはよいでしょう）

 2. **Scala IDE** の **ワークシート (worksheet)** を使用する（IDE が好きな人にはよいでしょう）

ここでは、それらを準備するための各工程を一通り説明していきます。

## Scala コンソールを準備する

[http://scala-lang.org](http://scala-lang.org) の手順に従って、コンピューターに Scala を設定します。一度 Scala がインストールされれば、コマンドラインプロンプトで `scala` と入力することで、対話的なコンソールを実行できるようになるはずです。これは macOS の例です。（訳注：内容を2020年現在のものに修正しています。）

```bash
dave@Jade ~> scala
Welcome to Scala 2.13.1 (OpenJDK 64-Bit Server VM, Java 1.8.0_242).
Type in expressions for evaluation. Or try :help.

scala>
```

`scala>` プロンプトで個々の式を入力し、**Enter** キーを押下することで、それらをコンパイルし、実行することができます。

```tut
"Hello world!"
```

### 一行の式を入力する

単純な式を入力してみましょう。

```tut
1 + 2 + 3
```

Enter キーを押下したとき、コンソールは3つのことを応答します。

 - **識別子 (identifier)** `res1`
 - **型 (type)** `Int`
 - **値 (value)** `6`

次章で見ていくように、Scala のすべての式は **型** と **値** を持ちます。型はコンパイル時に決定され、値は式を実行するときに決定されます。ここでは、その両方が報告されています。

識別子 `res1` は、式の結果を将来の式の中で参照できるように、コンソールが提供する便利なものです。例えば、下記のように先ほどの結果を2倍することができます。

```tut
res1 * 2
```

有用な値を生成しない式を入力すると、コンソールは応答として何も印字しません。

```tut
println("Hello world!")
```

ここで、`"Hello world!"` という出力は `println` という記述に由来しています。入力した式は実際のところ値を返しません。コンソールは上記で見た出力に類するものを提供していないのです。

### 複数行の式を入力する

簡単に複数行に渡る長い式を分割することができます。式が終わる前に Enter キーを入力すると、コンソールは次の行に継続していることを示す `|` という文字を印字します。

```tut
for(i <- 1 to 3) {
  println(i)
}
```

一度に複数行の式を入力したいことがあります。このような場合、`:paste` コマンドを使用することができます。単純に `:paste` と入力し、Enter キーを押下し、コードを記述もしくはコピー＆ペーストしてください。すべてを入力し終わったら `Ctrl+D` を押下してください。通常のようにコードのコンパイルと実行がされます。

```scala
scala> :paste
// Entering paste mode (ctrl-D to finish)

val x = 1
val y = 2
x + y

// Exiting paste mode, now interpreting.

x: Int = 1
y: Int = 2
res6: Int = 3
```

ファイルに Scala コードがあるのであれば、ファイルの内容をコンソールの中に貼り付けるために `:paste` を使用できます。これは、コンソールに式を再入力するよりはるかに便利です。例えば、`1 + 2 + 3` を含む `example.scala` という名前のファイルがあるとき、下記のように `:paste` を使用できます。

```scala
scala> :paste example.scala
Pasting file example.scala...
res0: Int = 6
```

### 式の型を印字する

コンソールを使用する上での最後のテクニックです。時々、式を実際に実行せずに、その**型**を知りたいことがあります。それをするために、`:type` コマンドを使用することができます。

```scala
scala> :type println("Hello world!")
Unit
```

コンソールは、この式の `println` という記述を実行しないことに注意してください。コードをコンパイルし、この場合は `Unit` と呼ばれる何らかの型を印字しているだけです。

Scala の `Unit` は Java や C の `void` と同じです。詳細は第2章を読んでください。

## Scala IDE を準備する

<div class="callout callout-warning">
#### Scala IDE は非推奨です（訳者追記） {-}

Scala IDE は、2018年1月に 4.7.1 RC3 がリリースされたのを最後に更新がありません。そのため、訳書においては Scala IDE の利用を非推奨とし、本節を翻訳していません。

なお、Scala の IDE としては [IntelliJ IDEA](http://www.jetbrains.com/idea/) が主流になっています。**ワークシート**の機能もありますので、IDE を利用したい場合は IntelliJ IDEA をインストールすることで代替できると思います。しかし、訳書においては Scala コンソールでのみ検証しておりますので、Scala コンソールの利用を強く推奨します。
</div>

*Scala IDE* is a plugin that adds Scala language support to [Eclipse](http://eclipse.org). A complete version of Scala IDE with Eclipse is also available as a standalone bundle from [http://scala-ide.org](). This is the easiest way to install the software so we recommend you install the standalone bundle for this course.

Go to [http://scala-ide.org](http://scala-ide.org) now, click the **Get the Bundle** button, and follow the on-screen instructions to download Scala IDE for your operating system:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Main website](src/pages/getting-started/scala-ide-website.png)\


Once you have downloaded and uncompressed the bundle you should find an application called **Eclipse**. Launch this. You will be asked to choose a folder for your *workspace*:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Choose a workspace location](src/pages/getting-started/scala-ide-workspace-chooser.png)\


Accept the default location and you will see an empty main Eclipse window:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Empty workspace](src/pages/getting-started/scala-ide-empty-workspace.png)\


### Creating your First Application

Your *Eclipse workspace* is two things: a folder containing files and settings, and a main window where you will be doing most of your Scala programming. In your workspace you can find *projects* for each Scala application you work on.

Let's create a project for the book exercises now. Select the **File menu** and choose **New > Scala Project**:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Create a new Scala project](src/pages/getting-started/scala-ide-new-project.png)\


Enter a **Project name** of `essential-scala` and click **Finish**. The tree view on the left of your workspace should now contain an empty project:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Empty project](src/pages/getting-started/scala-ide-empty-project.png)\


A project is no good without code to run! Let's create our first simple Scala application - the obligatory *Hello World* app. Select the **File Menu** and choose **New > Scala Object**:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Create a Scala object](src/pages/getting-started/scala-ide-new-object.png)\


**Name** your object `HelloWorld` and click **Finish**. A new file called `HelloWorld.scala` should appear in the tree view on the left of the main window. Eclipse should open the new file in the main editor ready for you to start coding:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Single Scala file](src/pages/getting-started/scala-ide-single-file.png)\


The content of the file should read as follows:

```tut:book:silent
object HelloWorld {

}
```

Replace this text with the following minimalist application:

```tut:book:silent
object HelloWorld {
  def main(args: Array[String]): Unit = {
    println("Hello world!")
  }
}
```

Select the **Run Menu** and choose **Run**. This should execute the code in your application, resulting in the words `Hello world!` appearing in the *Console* pane at the bottom of the window. Congratulations - you just ran your first Scala application!

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Hello World](src/pages/getting-started/scala-ide-hello-world.png)\


Developers with Java experience will notice the resemblance of the code above to the Java hello world app:

```java
public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Hello world!");
  }
}
```

The resemblance is, of course, no coincidence. These two applications compile to more or less the same bytecode and have exactly the same semantics. We will learn much more about the similarities and differences between Scala and Java as the course continues.

### Creating your First Worksheet

Compiling and running code whenever you make a change is a time consuming process that isn't particularly suitable to a learning environment.

Fortunately, Scala IDE allows us to create special files called *Scala Worksheets* that are specifically designed for training and experimentation. Every time you save a Worksheet, Eclipse automatically compiles and runs your code and displays the output on the right-hand side of your editor. This provides instant feedback, which is exactly what we need when investigating new concepts!

Create your first Scala Worksheet by selecting the **File Menu** and choosing **New > Scala Worksheet**:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: New Scala worksheet](src/pages/getting-started/scala-ide-new-worksheet.png)\


Enter a **Worksheet name** of `FirstSteps` and click **Finish**. A new file called `FirstSteps.sc` should appear in the tree view on the left of the main window, and should open it in the main editor in the middle:

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Empty Scala worksheet](src/pages/getting-started/scala-ide-empty-worksheet.png)\


Note that the object on the left contains a single line of Scala code:

```tut:book:silent
println("Welcome to the Scala worksheet")
```

for which Eclipse is displaying the corresponding output on the right:

```
Welcome to the Scala worksheet
```

Any expression you add to the left of the editor is evaluated and printed on the right. To demonstrate this, change the text in the editor to the following:

```scala
object FirstSteps {
  println("Welcome to the Scala worksheet")

  1 + 1

  if(20 > 10) "left" else "right"

  println("The ultimate answer is " + 42)
}
```

Save your work by selecting the **File Menu** and choosing **Save** (or better still by pressing **Ctrl+S**). Eclipse should automatically evaluate each line of code and print the results on the right of the editor:

```scala
object FirstSteps {
  println("Welcome to the Scala worksheet")   //> Welcome to the Scala worksheet

  1 + 1                                       //> res0: Int(2) = 2

  if(20 > 10) "left" else "right"             //> res1: String = left

  println("The ultimate answer is " + 42)     //> The ultimate answer is 42
}
```

<!-- Trailing slash and double newline are REQUIRED to prevent LaTeX repositioning this -->
![Scala IDE: Completed Scala worksheet](src/pages/getting-started/scala-ide-completed-worksheet.png)\


We'll dive into what all of the text on the right means as we proceed with the course ahead. For now you're all set to start honing your Scala skills!
