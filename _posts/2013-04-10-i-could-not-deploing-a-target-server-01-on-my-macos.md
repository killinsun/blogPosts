---
ID: 172
post_title: >
  HackerJapan
  2013年５月号のやられサーバー01がMacで起動できなかった話
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2013/04/i-could-not-deploing-a-target-server-01-on-my-macos/
published: true
post_date: 2013-04-10 01:52:16
---
&nbsp;
<div class="section">

先日、発売されたばかりのHackerJapan２０１３年５月号を購入。
（ついでに日経Linuxも）

去年９月あたりから買い始めて
今回は収録DVDにやられサーバーが３つ収録されてた。

試しにMacのVirtualBOXに入れてみようとしたんだけど
[yarare02]はすんなりとインポートできたけど
[yarare01]はわけのわからんエラーが出てインポートできない。

どういう事かと思って、 ls -lで権限みてみたら
書き込み権限がなかった。

chmod 755 FreeBSD*

とやってみて上手くインポートして起動する事ができた。

windowsだと問題なくいけたんだろうか？
<h5>表記ミス?</h5>
P25のNessusを起動するの項、３番のアクティベーションコードを入手したら
[nessus-register --fetch]コマンドを入力する　　ってあるけど
スクリーンショットだと　nessus-fetch --register　になってる。
後者が正解っぽい。

</div>