---
ID: 162
post_title: 'Googleアカウントを移行する&amp;コピーする'
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2013/01/migrate-google-account-and-copy/
published: true
post_date: 2013-01-04 13:31:48
---
<div class="section">

Googleでメインとなるアカウントを作り直したんですが、
普段使ってるChromeのブックマークやプラグイン、デザインをそっくりそのまま
新しいGoogleアカウントに移行させる方法はないかなーと試したのでメモ。

最初の２ステップはぶっちゃけ必要ないっぽいけど、同期事故を防ぐため、念のため。


<span class="deco" style="font-size: large;">1Chromeブラウザ内にユーザーを新規追加する</span>
<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130104130904" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130104130904j:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130104/20130104130904.jpg" alt="f:id:killinsun:20130104130904j:image" /></a>
画像や名前はわかればなんでもいい。
新規追加後、新しくChromeが別窓で起動し、Googleアカウントのログイン画面に入る。
ログインしてもしなくてもいい。
これで、ブラウザ内（コンピュータ内）に空っぽのデータ、旧のデータが二つあることになる。

<span class="deco" style="font-size: large;">2旧アカウントの設定ファイルを新アカウントに丸ごとコピー</span>

C:Usersログオンユーザー名AppDataLocalGoogleChrome
Applicationにはブラウザそのもののファイルが入ってるらしい。
UserData内に設定ファイルは入っている。

UserDataフォルダ内にはいくつかフォルダとファイルがあるが、注目するのは二つのファイル
<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130104133304" target="_blank" rel="noopener noreferrer"><img class="hatena-fotolife" title="f:id:killinsun:20130104133304j:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130104/20130104133304.jpg" alt="f:id:killinsun:20130104133304j:image" /></a>
DefaultUser普段使っているユーザー（旧アカウント）
Profile(数字) が、新規追加したユーザー。（新アカウント）


Chromeを一旦終了し、DefaultUserフォルダの中身をコピーし、Profile(数字）内に全て上書きする形でコピーする。

これで、旧アカウントのデータが二つ複製された。

<span class="deco" style="font-size: large;">3実際にデータを移行させる</span>
Chromeを起動する。
ここで新アカウントでなければ、アカウントを切り替える。
メニュー→設定→Googleアカウントを切断する→Googleにログイン
新アカウントのユーザー名とパスワードの入力が求められるので、入力する。

現在のブラウザの情報が新アカウントのほうへ同期される。


プロセス的に３番目だけで十分なのかなーとは思ったけど
こういうのはミスった時が怖いので、一応ユーザーを複製してからやるやり方をお勧めします。

</div>