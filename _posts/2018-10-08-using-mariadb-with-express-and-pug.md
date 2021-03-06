---
ID: 128
post_title: >
  express +
  pugを使ってMariaDB(MySQL)と連携する
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2018/10/using-mariadb-with-express-and-pug/
published: true
post_date: 2018-10-08 23:54:42
---
<h3>前提</h3>
<ul>
 	<li>予めexpress-generatorなどを用いてexpress環境が出来ている事</li>
 	<li>MySQL環境（データベース、テーブル）は準備済みである事</li>
 	<li>pugのレイアウト分け、インポート等はなんとなく理解した上で分けている事</li>
</ul>
<h3>テストテーブル</h3>
手始めにこんな感じのシンプルなテーブルを用意した。

| id | name | other_name|
|---|--------|---------------|
|1|tony_stark|IRON-MAN|
|2|steve_rogers|Captain America|
<h3>下準備</h3>
<h4> mysqlパッケージインストール</h4>
npmでmysqlパッケージをインストール
<pre class="lang:default decode:true">npm install --save mysql</pre>
<h4>node環境変数にMySQL接続情報をセット</h4>
予めnode_envにユーザ名やパスワードを格納しておく。
node.jsの起動時やdotenvを使って記録しておくなど何でもよい。
詳しいことはググって。次の内容からいきなり読み出しています。
<h3>app.js</h3>
<pre class="lang:js decode:true">let bodyParser    = require('body-parser');
let createError   = require('http-errors');
let express       = require('express');
let path          = require('path');
let cookieParser  = require('cookie-parser');
let logger        = require('morgan');
let indexRouter   = require('./routes/index');
let userRouter    = require('./routes/user');
let mysql         = require('mysql');
let app           = express();

//--------- ①Database settings-------------
const db_conf = {
  host     : 'データベース接続先FQDN or IP addr',
  user     : process.env.MYSQL_USER,
  password : process.env.MYSQL_PASS,
  database : 'test_database'
}
const pool = mysql.createPool(db_conf);
app.set('pool', pool);

//----------------------------------------


app.use(bodyParser.urlencoded({extended: true}));

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'pug');

app.use(logger('dev'));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(express.static(path.join(__dirname, 'public')));

app.use('/', indexRouter);
app.use('/user', userRouter);

〜〜〜省略〜〜〜
module.exports = app;
</pre>
&nbsp;

まず、①でデータベースに接続するコンフィグを定義し、mysql.createPoolによってPoolオブジェクトを生成している。app.jsのこの位置に記載する事によってどのリクエストもデータベースとコネクションを開始する。

場合によっては記載する位置を変えて別モジュールから読み出させるなどで、必要に応じてデータベースにつなぎに行く回数を減らしてもいいのかもしれない。というかそうするべきである。

app.setによって各ルーターからコネクションに介入する事が出来る。

&nbsp;
<h3>index.js</h3>
<pre class="lang:js decode:true">'use strict';
require('dotenv').config();
const express = require('express');
const router = express.Router();
const date = require('date-utils')

/* GET home page. */
router.get('/', function(req, res, next) {

  //-------- ②リクエストからDBのコネクション情報を読取る--------
  const app   = req.app;   
  const pool  = app.get('pool');

  //---------③クエリを発行 -&gt; テンプレートにレンダリング ------
  pool.query('SELECT * FROM test_table;', function(error, results, fields){
    if(error) throw error;
    res.render('index', { title: 'DB Connect　Test,　results: results });
  });
  

});

module.exports = router;</pre>
続いてindexルーター。
②は、①の最後でセットしておいたコネクション情報を読み出し。

③でqueryメソッドを実行する。コールバック関数の２つ目「results」に結果が格納されるので、res.renderの第二引数のオブジェクトに設置してテンプレートエンジンにレンダリングさせる。
<h3>index.pug</h3>
最後にフロントエンド側。 node.jsでこねくり回したデータ、オブジェクトはここで表示する。
#{results}とすれば[Object object]としか表示されないので、一件ずつ解いていくやり方にした。
<pre class="lang:css decode:true">extends layout
block contents
  h1.title= title
  p.subtitle Welcome to #{title}
  div.columns
    each row in results  /* ここでループ。DBのresultsで返された件数分rowに格納していく。 */
      div.box
        p.row__id ID: #{row.id}　　/* rowに渡されたDBカラムの内容は row.{カラム名}で取得できる */
        p.row__name Name:  #{row.name}
        p.row__other_name Other_name: #{row.other_name}
</pre>
&nbsp;

pugの書き方で　each ~ inというのがあって、node.jsでいう　for ~ of と同じニュアンスで使えるような記法がある。今回はそれを使って pugで受け取っているオブジェクトを１つずつ取り出し、HTMLに吐き出す事とした。

&nbsp;

<hr />

今までサーバサイドからフロントエンドにデータを受け渡しするのはsocket.ioにJSONオブジェクトとして乗っけてしまえる所に頼ってしまっていたので、res.renderを使ってpugに表示させるやり方の理解に躓いた。

pugもpugで、javascriptが埋め込める分、console.log()とか使ってデバッグをしてもサーバ側でプリントされてしまうのでブラウザコンソールで表示させるのはどうしたらええんやと躓いた。

/以上

&nbsp;

&nbsp;