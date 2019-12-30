---
ID: 34
post_title: '【WEB開発メモ#1】 CentOS7系にnode.jsとExpress 4をインストールする'
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2018/05/installing-nodejs-and-express4-with-centos7/
published: true
post_date: 2018-05-07 23:05:17
---
node.jsを使ったWEB開発を行う為に、必要な手順を個人的なメモレベルで書き記していこうと思います。

WEB開発が本業ではないので、その筋の方々からすると遠回りだったり、ベストプラクティスではない内容だったりする場合があります。　あくまでチラシの裏です。

とはいえ、今後、職場で教育する為の資料作りのインプット（か、もしくはこれをそのまま投げつける形）にしたいなと思っています。

&nbsp;

<hr />

<h2>書いていくであろう内容（随時更新）</h2>
<ol>
 	<li>CentOS7にnode.jsとExpressをインストールする　←いまここ</li>
 	<li>Expressの基礎を学んでみる</li>
 	<li>テンプレートエンジンを用いてフロントエンドのHTMLを作成する</li>
 	<li>随時追加</li>
</ol>
では、以下より本題

<hr />

<h2>用意する環境</h2>
<ul>
 	<li>RHEL 7.x系OS　minimalインストール版</li>
</ul>
今回は検証用なのでCentOSで進める。本番環境で扱う場合はぜひRHELを使用して頂きたいなと思います。
<h2>今回目指す環境</h2>
<ul>
 	<li>Node.js をなるべく最新かつシンプルにインストールする。</li>
 	<li>Node.jsのパッケージ管理システム「npm」を用いてExpress4をインストールする。
<ul>
 	<li>npmコマンドの一般的な使い方を知る</li>
 	<li>express-generatorを使ってシンプルかつお手軽に模範環境を用意する</li>
 	<li></li>
</ul>
</li>
</ul>
<h2>手順</h2>
<h3>Node.jsのインストール</h3>
<h4>yumでインストールする際は注意</h4>
いきなりNode.jsをインストールします。

2018年5月現在、CentOSのepel-releaseリポジトリで「nodejs」を検索すると、

以下のような情報が返ってきます。
<pre class="lang:sh decode:true">[root@dev]# yum info nodejs
Loaded plugins: fastestmirror, langpacks
Loading miAvailable Packages
Name : nodejs
Arch : x86_64
Epoch : 1
Version : 6.14.0
Release : 1.el7
Size : 4.6 M
Repo : epel/x86_64
Summary : JavaScript runtime
URL : http://nodejs.org/
License : MIT and ASL 2.0 and ISC and BSD
Description : Node.js is a platform built on Chrome's JavaScript runtime
 : for easily building fast, scalable network applications.
 : Node.js uses an event-driven, non-blocking I/O model that
 : makes it lightweight and efficient, perfect for data-intensive
 : real-time applications that run across distributed devices.rror speeds from cached hostfile
</pre>
6.x系は古すぎるので、今回はLTS版の8.11.1`をインストールしたいと思います。
<h4>NodeSourceからインストール</h4>
この手順では、 <a href="https://github.com/nodesource/distributions" target="_blank" rel="noopener noreferrer">NodeSource</a> から、結果的に目的のnode.jsをインストールします。

今回は8.11.1をインストールするので、8系です。

以下コマンドを実行します。
<pre class="lang:default decode:true">[root@dev]# curl -sL https://rpm.nodesource.com/setup_8.x | bash -</pre>
その他のバージョンをインストールする場合は、上記リンクから目的のLinuxディストリビューションに合ったコードを探して実行してください。

その後、yumコマンドによってnodejsパッケージがインストール出来るようなるので、実行します。
<pre class="lang:default decode:true">[root@dev]# yum -y install nodejs</pre>
インストール後、以下のようにバージョンを確認できます。
<pre class="lang:default decode:true ">[root@dev]# node --version
v8.11.1</pre>
また、同時にNode.jsのパッケージ管理ツールである  npm もインストールされています。
<pre class="lang:default decode:true ">[root@dev]# npm --version
5.6.0</pre>
以降は、このnpmコマンドを使ってnode.jsにパッケージを追加していきます。

&nbsp;
<h3>npmコマンドを使ってみる</h3>
まず、npmコマンドを学ぶ為専用のディレクトリを用意しましょう。

今回は /usr/local/share/　に work という作業用ディレクトリを作成しました。

作業用ディレクトリを作成したのには、理由があります。

npmコマンドでは、システム全体にデフォルトで利用出来るようにする為の「グローバルモード」と、特定のディレクトリ配下のプロジェクトで有効な「ローカルモード」という概念があります。

同じパッケージで異なるバージョンがインストールされている場合、ローカル環境にインストールされている方が優先されます
（厳密言うと、.jsファイルでモジュールを読み込む場合、指定したモジュールがどこに配置されているのか自動で探索する仕組みがあるのですが、、、まぁそんなもんだと捉えてください）
<h4>npm init  --&gt; package.jsonの作成</h4>
パッケージをインストールする前に、一つ行う事があります。

今回作成した「 work」ディレクトリに仮のWEB開発プロジェクトに必要なリソース（モジュールだったり、サーバサイドのプログラムだったり、HTMLやCSSだったり）を格納するとしましょう。
npmによってインストールしたパッケージも、この中に格納されますが、githubなどでソースを公開する際、これらの外部から取得したパッケージも一緒にpushするのはよろしくありません。

package.jsonには、プロジェクトに必要な情報と、「必要なパッケージ、モジュール」や「起動時のスクリプト」などを記入する事ができます。

いきなりゼロからpackage.jsonを作成するのは心が折れますし、普段手動で手を加えないものなので、コマンドで作成しましょう。

npm initコマンドを使います。 対話型のコマンドですが、後で手動でも書き換えられるので、Enter押しっぱなしでも問題ありません。
<pre class="lang:default decode:true">[root@dev work]# npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install &lt;pkg&gt;` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (work)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
keywords:
author:
license: (ISC)
About to write to /usr/local/share/work/package.json:

{
  "name": "work",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo "Error: no test specified" &amp;&amp; exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this ok? (yes)</pre>
下部にあるJSON形式の文字列（ { "name": "work"から始まる部分）が、package.jsonの内容です。
準備はこれでOKです。
<h4>グローバルモードでインストール</h4>
まずは試しに、グローバルモードでExpress4をインストールしてみます。
<pre class="lang:default decode:true ">[root@dev work]# npm install --save -g express4
+ express4@0.0.1
added 51 packages in 2.425s</pre>
--save というのは、実行したカレントディレクトリ上の package.jsonに「依存関係のあるパッケージ」として記録する事を意味します。（--save-devも同様です）

-g をつけることで、グローバルモードでインストールされます。

デフォルトではグローバルモードのインストール先は　/usr/lib内にあるので、express4がインストールされているのが確認できます。
<pre class="lang:default decode:true ">[root@dev work]# ls /usr/lib/node_modules/
express4  npm</pre>
&gt; npmのデフォルト設定がどうなっているかを確認するには、**npm config list -l** コマンドで確認できます
<h4>ローカルモードでインストール</h4>
次に、ローカルモードの挙動を試します。

ちょっと捻くれてexpress3をインストールします。
ただし、今回はpackage.jsonに依存関係のあるパッケージとして記録するとややこしくなってしまうので、避けます。
<pre class="lang:default decode:true">[root@dev work]# npm install express3
npm WARN deprecated connect@2.30.2: connect 2.x series is deprecated
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN work@1.0.0 No description
npm WARN work@1.0.0 No repository field.

+ express3@0.0.0
added 96 packages in 3.448s
</pre>
実行結果を確認します。

まず、上から順に** npm list**コマンドを使って、ローカル環境のインストール済みパッケージを確認しました。

--depth=0 というのは、出力結果がツリー型表示で一番浅い階層（＝０階層目）で出力する事を意味します。

ローカル環境ではexpress3が読み込まれている事がわかります。

-g をつけたグローバル環境では、express4が読み込まれています。（加えて、npmもインストールされていますね）
<pre class="lang:default decode:true">[root@dev work]# npm list  --depth=0
work@1.0.0 /usr/local/share/work
└── express3@0.0.0

[root@dev work]# npm list  --depth=0 -g
/usr/lib
├── express4@0.0.1
└── npm@5.6.0


[root@dev work]# ls node_modules/
accepts              cookie-signature  forwarded          morgan           send
base64-url           core-util-is      fresh              ms               serve-favicon
basic-auth           crc               http-errors        multiparty       serve-index
basic-auth-connect   csrf              iconv-lite         negotiator       serve-static
batch                csurf             inherits           on-finished      statuses
body-parser          debug             ipaddr.js          on-headers       stream-counter
bytes                depd              isarray            parseurl         string_decoder
commander            destroy           media-typer        pause            tsscmp
compressible         ee-first          merge-descriptors  proxy-addr       type-is
compression          errorhandler      method-override    qs               uid-safe
connect              escape-html       methods            random-bytes     unpipe
connect-timeout      etag              mime               range-parser     utils-merge
content-disposition  express           mime-db            raw-body         vary
content-type         express3          mime-types         readable-stream  vhost
cookie               express-session   minimist           response-time
cookie-parser        finalhandler      mkdirp             rndm</pre>
最後の**ls**コマンドでは、ローカル環境でインストールされている細かいモジュール群を表示しています。パッケージのソースコードを直接確認する場合、このようにnode_modulesディレクトリから辿っていく事になります。

&nbsp;
<h4>ローカル環境のexpress3をアンインストール</h4>
グローバルモードとローカルモードの違いと、使い分けが理解できたところで、ややこしいexpress3はアンインストールします。
<pre class="lang:default decode:true ">[root@dev work]# npm uninstall express3
npm WARN work@1.0.0 No description
npm WARN work@1.0.0 No repository field.

removed 96 packages in 1.136s</pre>
uninstallサブコマンドでアンインストールが完了します。(もちろん、-gをつけた場合はグローバルが対象になります）

上述したコマンドを駆使してアンインストールされているか確認してみてください。

また、package.jsonからも記載を削除する場合、uninstallサブコマンドの後に --saveもしくは --save-devを追記する事で削除できます。

npmコマンドの基本的な使い方は以上です、
<h3>express-generatorを使う</h3>
expressを使うにあたって、必要なファイル、ディレクトリの真っさらなテンプレートを自動で生成してくれるのがexpress-generatorです。

これもパッケージなので、覚えたばかりのnpmコマンドを使ってインストールします。
*ローカルインストールではなく、グローバルインストールしてください。パスを通さないとexpressコマンドが使えません。
<pre class="lang:default decode:true">[root@dev work]# npm install -g express-generator
npm WARN work@1.0.0 No description
npm WARN work@1.0.0 No repository field.

+ express-generator@4.16.0
added 10 packages in 0.843s</pre>
&nbsp;

次に、expressで管理する最大単位となる「アプリケーション」を配置するディレクトリを決めます。

今回は*/usr/local/share/first-project* にしたいと思います。
（まだディレクトリを作成する必要はありません。/usr/local/shareに移動だけしておいてください。）

では、expressコマンドを使ってアプリケーションを作成します。
<pre class="lang:default decode:true">[root@dev share]# express first-project

  warning: the default view engine will not be jade in future releases
  warning: use `--view=jade' or `--help' for additional options


   create : first-project/
   create : first-project/public/
   create : first-project/public/javascripts/
   create : first-project/public/images/
   create : first-project/public/stylesheets/
   create : first-project/public/stylesheets/style.css
   create : first-project/routes/
   create : first-project/routes/index.js
   create : first-project/routes/users.js
   create : first-project/views/
   create : first-project/views/error.jade
   create : first-project/views/index.jade
   create : first-project/views/layout.jade
   create : first-project/app.js
   create : first-project/package.json
   create : first-project/bin/
   create : first-project/bin/www

   change directory:
     $ cd first-project

   install dependencies:
     $ npm install

   run the app:
     $ DEBUG=first-project:* npm start

[root@dev share]# ls
applications  first-project  info  man  work</pre>
&nbsp;

いくつかディレクトリとフォルダが作成されました。

大まかな部分で役割を説明していきます。
<pre class="lang:default decode:true">.
├── app.js　　//メインとなるjavascriptファイル
├── bin `//サーバ起動に必要な実行ファイル
│   └── www　　
├── package.json 
├── public　//CSSやjs、imageファイルなどの静的コンテンツの配置場所
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes　//ルーティングされるアプリケーションはここに別記載する
│   ├── index.js
│   └── users.js
└── views　//画面描画用のテンプレート　今回は説明割愛
    ├── error.jade
    ├── index.jade
    └── layout.jade</pre>
なお、これらはジェネレーターが自動作成するテンプレートの一つにすぎず、必要に応じてディレクトリを作成してデータベーススキームを格納したり、そのほかのスクリプトファイルを配置したりする場合もあります。

また、express-generatorによって自動的に作成されたpackage.jsonがあるのに気が付いたでしょうか。
この中には、真っさらな状態のexpressを利用するために必要なパッケージが記載されています。
こいつを使って、依存関係のあるパッケージを全て自動でインストールしてみましょう。
<pre class="lang:default decode:true">[root@dev first-project]# npm install
npm WARN deprecated jade@1.11.0: Jade has been renamed to pug, please install the latest version of pug instead of jade
npm WARN deprecated constantinople@3.0.2: Please update to at least constantinople 3.1.1
npm WARN deprecated transformers@2.1.0: Deprecated, use jstransformer
npm notice created a lockfile as package-lock.json. You should commit this file.
added 101 packages in 3.203s</pre>
package.jsonに記載されている内容をそのまま反映させるには、**npm install**とだけ実行します。

これで、必要なパッケージがローカルインストールされました。
<h3>expressを使ってみる</h3>
express-generatorを使ってディレクトリやファイルを自動で配置し、更にnpmコマンドによって必要なパッケージを自動でインストールしたので、すでにサーバを起動する事が出来る状態です。

**npm start**コマンドでサーバを起動してみます。
<pre class="lang:default decode:true ">[root@dev first-project]# npm start

&gt; first-project@0.0.0 start /usr/local/share/first-project
&gt; node ./bin/www</pre>
この状態でプロンプトが返ってこなければnode.jsがサーバで起動しています。
大半はデフォルトで３０００番ポートが開いているので、
http://{fqdn}:3000/
に対してアクセスするとexpressのデフォルト画面が確認できます。

Welcome to Expressという文字列が表示されていれば成功です。

&nbsp;

ひとまず、CentOS7環境にNode.js + Expressの環境を、体系的に学びつつ構築する手順を記載しました。

次回はルーティングについて知識を整理してみようかなと思います。

&nbsp;

/以上