---
ID: 178
post_title: >
  Eclipse4.2でGUIアプリ開発の準備メモ
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/1970/01/build-an-environment-with-gui-application-on-eclipse-4-2/
published: true
post_date: 1970-01-01 00:00:00
---
&nbsp;
<div class="section">

部活内でチーム開発をすることになったので、環境構築をした。
予めHelloworldまで確認したので、メンバーの後輩へのドキュメント用にメモを残す。


環境は以下の通り。

言語：Java
IDE:Eclipse4.2
プラグイン:WindowBuilder(SWTDesigner等）
<h5>本体を入れる</h5>
まずはEclipse4.2を導入する
<a href="http://www.eclipse.org/downloads/" target="_blank" rel="noopener noreferrer">http://www.eclipse.org/downloads/</a>

日本語化とかは面倒くさいので今回はスルーで。
<h5>WindowBuilder周りを入れる</h5>
&nbsp;

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130525234448" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130525234448p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130525/20130525234448.png" alt="f:id:killinsun:20130525234448p:image" /></a>
ヘルプ→新規ソフトウェアのインストール


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130525234449" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130525234449p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130525/20130525234449.png" alt="f:id:killinsun:20130525234449p:image" /></a>
対象のところに
<a href="http://download.eclipse.org/releases/juno/" target="_blank" rel="noopener noreferrer">http://download.eclipse.org/releases/juno/</a>
このURLを入力。
4.2用の一般的なプラグインが表示される。

一般ツールカテゴリあたりから、
SWT DesignerとSWT Designer Coreをインストール。
フィルタって所で　SWTと入力すると絞り込んでくれて便利。


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130525234450" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130525234450p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130525/20130525234450.png" alt="f:id:killinsun:20130525234450p:image" /></a>

依存関係のあるパッケージは自動でインストールしてくれるらしい。
ぶっちゃけSWT Designerだけ選択でもよかったかも。
下手に何か入れると、後述のデザインモードが起動できなくなるバグっぽいのが発生する


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526000325" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526000325p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526000325.png" alt="f:id:killinsun:20130526000325p:image" /></a>
同意して完了をクリックでインストール開始。
この後再起動求められるので、素直に再起動を。
<h5>起動するか確認</h5>
&nbsp;

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526000327" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526000327p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526000327.png" alt="f:id:killinsun:20130526000327p:image" /></a>
適当なプロジェクトを作る。
馴染みの深いJavaプロジェクトではなく、今回は
WindowBuilder配下に<span class="deco" style="font-weight: bold;">SWT/JFace Java Project</span>というのがあるので、
これでプロジェクトを作成してみる。　必要なライブラリが自動で格納される。


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526001154" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526001154p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526001154.png" alt="f:id:killinsun:20130526001154p:image" /></a>
そしてクラスの作成。
今回は、同カテゴリ内のSWT Applicationsというのでクラスを作成してみた。


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526001156" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526001156p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526001156.png" alt="f:id:killinsun:20130526001156p:image" /></a>
作成したファイルを開いてみる。
ソースコードの下の方のタブをDesignに変えると、デザインモードに切り替わる。
<h5>Helloworldをやってみる</h5>
&nbsp;

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526001157" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526001157p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526001157.png" alt="f:id:killinsun:20130526001157p:image" /></a>
テキストやボタンなどの「パーツ」は、
それぞれ、ウィンドウに配置する際、レイアウトという「置き方」を決めなければならない。
今回は、FillLayoutというのを選択してみる。

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526001158" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526001158p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526001158.png" alt="f:id:killinsun:20130526001158p:image" /></a>
次にラベルの配置。
画面に埋め込む文字列ですね。隣のTextを選択すると、テキストフィールドが出てくる。


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526001159" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526001159p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526001159.png" alt="f:id:killinsun:20130526001159p:image" /></a>
配置した時に、Labelの文字を書き換える事ができるが、動きを理解するために
そのまま配置することにする。

ソースコードを見てみよう。
以下のソースコードは、上の手順まで進んだ結果、追加されたコードである。
<pre class="syntax-highlight"><span class="synType">protected</span> <span class="synType">void</span> createContents() {
shell = <span class="synStatement">new</span> Shell();
shell.setSize(<span class="synConstant">450</span>, <span class="synConstant">300</span>);
shell.setText(<span class="synConstant">"SWT Application"</span>);
shell.setLayout(<span class="synStatement">new</span> FillLayout(SWT.HORIZONTAL));
Label lblNewLabel = <span class="synStatement">new</span> Label(shell, SWT.NONE);
lblNewLabel.setText(<span class="synConstant">"New Label"</span>);
}
</pre>
&nbsp;

一番下の2行のコードが、追加したラベルに関するコードである。
1行目がインスタンス（実態）を作成し、2行目で、インスタンスの情報を設定している。
今回は、setText() というメソッドを使い、 New Labelという文字列が埋め込まれている。
ここを書き換えればよい。
<pre class="syntax-highlight">lblNewLabel.setText("Hello World!");
</pre>
&nbsp;

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130526002811" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130526002811p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130526/20130526002811.png" alt="f:id:killinsun:20130526002811p:image" /></a>
書き換えて実行すると、このようにHelloWorldが表示された。
Welcome to GUI world.　といった具合に。

</div>