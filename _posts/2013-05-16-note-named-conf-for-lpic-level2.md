---
ID: 176
post_title: LPIC Level2用　named.confメモ
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2013/05/note-named-conf-for-lpic-level2/
published: true
post_date: 2013-05-16 10:42:23
---
&nbsp;
<div class="section">

LPIC Level2を今日受験する予定だけど、
named.confの細かい所が理解できないので、直前メモ
<h5>ACL</h5>
&nbsp;
<blockquote>acl localnet{
192.168.0.0/24
}</blockquote>
&nbsp;

aclステートメントでネットワークやIPアドレスに名前をつける。
ここでは、 192.168.0.0/24ネットワークをlocalnetとして対応付けている
下記のステートメントなどで、ここで定義した名前をつけることができるようになる。

CCNAから入った身としては、アクセス制御リストとして
拒否設定を行うのかと勘違いしてた。
<h5>include</h5>
外部ファイルの読み込み
<blockquote>include "file";</blockquote>
実機で確認すると、鍵ファイルなどを読み込んでいる事が多かった。
<h5>controls</h5>
namedを操作する事ができるIPアドレスやポート番号を指定する。
さっきまでaclとごっちゃになってた。
<h5>options</h5>
&nbsp;

めちゃくちゃ多い。複雑すぎ。

<span class="deco" style="font-weight: bold;">-directory</span>
ゾーンファイルが格納されるディレクトリの絶対パス

<span class="deco" style="font-weight: bold;">-recursion</span>

yes または　no を指定することで、再帰的問い合わせを受け付けるかどうかを指定する。

再帰的問い合わせっていうのは、
自身が持っているドメインの情報以外のリクエストがきたときに「知らない」と答えるか。
知らないドメインの情報が出た場合に、どっか別のDNSサーバーからひっぱてきて答えることを
再帰的問い合わせという。

<span class="deco" style="font-weight: bold;">-recursive-clients</span>

再帰的問い合わせを受け付けるクライアントの数を指定する。そのまんま。
多いと鯖が死ぬ。

<span class="deco" style="font-weight: bold;">-forward</span>

上の再帰的と居合わせと被ってややこしい。
自身が名前の情報を持たずキャッシュにすら存在しない問い合わせがある場合、
他のDNSサーバーに問い合わせる事。

only かfirstで指定する。

only:フォワードによる名前解決のみを行い、自身では再帰問い合わせを行わない
first:自身が持っていない情報であれば、最初にフォワードによる名前解決を試す。

<span class="deco" style="font-weight: bold;">-forwarders{}</span>
ここでフォワードする宛先を指定する

</div>