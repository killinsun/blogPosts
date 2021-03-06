---
ID: 237
post_title: Go言語ちょっとだけ入門
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2018/11/getting-start-golang/
published: true
post_date: 2018-11-25 09:00:37
---
<h2>環境準備</h2>
VSCode上で適当なワークスペースを用意した。
今回は「/Users/{わたし}/Documents/Projects/golang_introduction
としてディレクトリを用意した。
<h3>golangのインストール</h3>
macOSを使用しているので、homebrewを使ってインストールする
<pre class="lang:default decode:true">$ brew info go
go: stable 1.11.1 (bottled), HEAD
Open source programming language to build simple/reliable/efficient software
https://golang.org
Not installed
From: /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/go.rb
==&gt; Requirements
Required: macOS &gt;= 10.10 ✔
==&gt; Options
--HEAD
	Install HEAD version
==&gt; Caveats
A valid GOPATH is required to use the `go get` command.
If $GOPATH is not specified, $HOME/go will be used by default:
  https://golang.org/doc/code.html#GOPATH

You may wish to add the GOROOT-based install location to your PATH:
  export PATH=$PATH:/usr/local/opt/go/libexec/bin
==&gt; Analytics
install: 91,529 (30d), 265,569 (90d), 943,528 (365d)
install_on_request: 64,960 (30d), 187,842 (90d), 594,605 (365d)
build_error: 0 (30d)

</pre>
&nbsp;

brewで入る最新は1.11.2だった。1.11自体が2018/08リリースと割と新しいものだったのでこのままインストール
<pre class="lang:default decode:true">$ brew install go</pre>
&nbsp;

VSCodeについては
.goファイルを作成してしまえばVSCodeからエクステンションのインストール案内が出るので
そのままインストール。
<h2>入門</h2>
<h3>GOPATHの設定</h3>
<pre class="lang:default decode:true ">export GOPATH=/Users/{わたし}/Documents/Project/golang_introduction/</pre>
golangはワークスペースプロジェクト単位で開発する。
基本的にコマンドやビルドを行う際にGOPATHを頼りに行うイメージかな？
ディレクトリ(=プロジェクト)毎にGOPATHを設定したい等のニーズに答えるために<a href="https://bitbucket.org/ymotongpoo/goenv">goenv</a>といったツールも用意されている。
<h3>Hello world</h3>
<pre class="tab-size:2 lang:go decode:true" title="hello_world.go">package main

import(
		"fmt"
)

func main() {
		fmt.Println("Hello world!")
}</pre>
<h4>実行</h4>
<pre class="lang:default decode:true">$go run hello_world.go
Hello world!</pre>
<h4>疑問：importしている「fmt」って何？</h4>
フォーマットから取ったパッケージ。
基本的にはC言語でいう「print」や 「scan」にあたる関数を提供する。
<h3>コメント</h3>
<pre class="lang:go decode:true ">// 1行コメント

/*
複数行コメント

*/</pre>
&nbsp;
<h3>import</h3>
<pre class="lang:go decode:true">import (
	"fmt"
	"math"
)</pre>
こっちも可
<pre class="lang:go decode:true">import "fmt"
import "math"</pre>
ただし、こちらは推奨されないらしい。

&nbsp;
<h3>exported name</h3>
golangでは名前の先頭が大文字のものは、外部から利用できる。
<pre class="lang:go decode:true">package main

import (
	"fmt"
)

func main() {
	fmt.Println("Hello world!")
}</pre>
この場合だとPrintlnがそれにあたる。
<h3>変数の定義</h3>
var で宣言して初期化する事が出来る。
型を明示的に指定する場合、intなどの型名は<strong>変数名の後</strong>に指定する。
<pre class="lang:go decode:true">var x int = 10
var y int = 20

//こっちも可能
var x,y int = 10,20
</pre>
文字列の場合はダブルクォーテーションで括る
<pre class="lang:go decode:true">var foo string = "bar"
fmt.Println(foo) //bar
</pre>
&nbsp;

また、上記より短い宣言もできる
<pre class="lang:default decode:true ">var foo = "bar"</pre>
varという初期化子をつけて変数に値を代入した場合、型を省略できる。
省略された変数の型は、代入された値によって自動的に割り当てられる。
（型推論）

他に、:=を使って宣言する方法がある。
<pre class="lang:go decode:true">foo2 := "bar"

var num1 = 10
num2 := 100</pre>
:= を使って宣言するのは型などをいい感じに認識してもらう「暗黙的宣言」という。
「暗黙的宣言」の場合は関数スコープ内でのみ利用が可能である。
つまり、関数の外では:=を使った暗黙的宣言はできない。
<h4>定数</h4>
最近のJavaScriptのように「const」をつけることで定数が宣言できる
<pre class="lang:go decode:true">const message = "hello?"

message = "Hi!" //エラー</pre>
定数（const）は、文字、文字列、boolean、数値でのみ使える。
定数の場合は　:= を使った宣言は利用できない。
<h4>ゼロ値</h4>
変数宣言時、何も値を指定しない場合、ゼロ値（zero value)が与えられる。
ゼロ値といっても数字の０（ゼロ）を入れるだけでなく、変数型によって異なる
<ul>
 	<li>数値型の場合(int/float) ... 0</li>
 	<li>bool型の場合... false</li>
 	<li>string型の場合... "" (空文字列）</li>
</ul>
この辺りはnull判定のときなどに活用できそうだ。
<h3>関数,function</h3>
よくある感じでOK。引数を指定する場合は上述の通り型が後。
<pre class="lang:go decode:true">//func( [引数名] [型])　[戻り値の型] {}

func main() {
	calc(1,2) //3
}

func calc(a int, b int){
	var c int
	c = a + b
	fmt.Println(c)
}</pre>
<h4>マルチプルリザルト</h4>
golangの特徴で、関数が複数の戻り値を返す事ができる。
<pre class="lang:go decode:true">func main() {
    a, b := swap("hello", "world")
    fmt.Println(a, b)  // hello__x world~~y
}

func swap(x, y string) (string, string){
    x += "__x"
    y += "~~y"
    return x, y
}</pre>
<h3>if文</h3>
条件式は丸括弧（）で括らない。
<pre class="lang:go decode:true">func main() {
	evaluation("test","test") //a==b
	evaluation("test","hoge") //a!=b
}


func evaluation(a string, b string){

	if a==b {
		fmt.Println("a==b")
	}else{
		fmt.Println("a!=b")
	}
}</pre>
<h3>for文</h3>
よくあるfor文はこんな感じ。他の言語のように丸かっこ（）で括る必要はない点に注意
<pre class="lang:go decode:true">for i :=0; i &lt; count; i++ {
	fmt.Println(i)
}

//0
//1
//2
//3
//4
//5
//6
//7
//8
//9</pre>
他の言語でいうwhileっぽいやり方も出来る
<pre class="lang:go decode:true">var i = 0
for i &lt; count {
	i++	
	fmt.Println(i)
}

//0
//1
//2
//3
//4
//5
//6
//7
//8
//9</pre>
<h4>無限ループ</h4>
<pre class="lang:go decode:true ">for {
  fmt.Println("forever!")
}</pre>
&nbsp;
<h3>Array, 配列</h3>
配列は固定長。長さを変える事ができない。
<pre class="lang:go decode:true">func main() {
	var a [2]string
	a[0] = "Captain"
	a[1] = "America"
	fmt.Println(a[0], a[1]) // Captain America
	fmt.Println(a) // [Captain America]

	primes := [6]int{2, 3, 5, 7, 11, 13}
	fmt.Println(primes) //[2 3 5 6 11 13]
}</pre>
個人的には変数そのものを出力するとカンマ区切りでなく
スペース区切りになる点が気になる

&nbsp;
<h3>スライス</h3>
こっちは可変長の配列だと思っていいらしい
<pre class="lang:go decode:true">func main() {

	names := [4]string{
		"John",
		"Paul",
		"George",
		"Ringo",
	}
	fmt.Println(names) // [Joh Paul George Ringo]

	a := names[0:2]
	b := names[1:3]
	fmt.Println(a, b) // [John Paul]  [ Paul George]  ...①

	b[0] = "XXX"
	fmt.Println(a, b)  // [John XXX] [ XXX George]　　...②
	fmt.Println(names) // [John XXX George Ringo]
}
</pre>
他の言語もそうなのかもしれないけど、スライスは以下のような形で切り取って扱う事が出来る
<table>
<tbody>
<tr>
<th>0</th>
<td>John</td>
<th>1</th>
<td>Paul</td>
<th>2</th>
<td>George</td>
<th>3</th>
<td>Ringo</td>
</tr>
</tbody>
</table>
①では、最初に０〜２を指定している。つまり、以下の範囲を指定したことになる。
つまり、John とPaulが出力対象
<table>
<tbody>
<tr>
<th>0</th>
<td>John</td>
<th>1</th>
<td>Paul</td>
<th>2</th>
</tr>
</tbody>
</table>
次に、１〜３を指定している。つまり、以下の範囲で、PaulとGeorgeが取扱の対象。
<table>
<tbody>
<tr>
<th>1</th>
<td>Paul</td>
<th>2</th>
<td>George</td>
<th>3</th>
</tr>
</tbody>
</table>
<h4>スライスは配列への参照のようなもの　（らしい）</h4>
<blockquote>スライスは配列への参照のようなものです。
スライスはどんなデータも格納しておらず、単に元の配列の部分列を指し示しています。
スライスの要素を変更すると、その元となる配列の対応する要素が変更されます。
同じ元となる配列を共有している他のスライスは、それらの変更が反映されます。</blockquote>
②で取り扱っているのがまさにその部分。　一度スライスを他の変数に代入し、他の編集の値を書き換えたとしても、大元のスライスにもその変更が反映される。

&nbsp;

疲れたので一旦ここまで

&nbsp;

&nbsp;