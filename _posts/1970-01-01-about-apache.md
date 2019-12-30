---
ID: 177
post_title: Apacheの話
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/1970/01/about-apache/
published: true
post_date: 1970-01-01 00:00:00
---
<br>
<div class="section">
<p>httpd.confのプロセス関連の件について、わかりづらい所があった。<br>ので、まとめる。<br>間違えている点があれば指摘して欲しいです。<br></p>
<ul>
<li>StartServers :Apache起動時の子プロセス数</li>
<br>				<li>MinSpareServers :待機プロセスの最小数</li>
<li>MaxSpareServers :待機プロセスの最大数</li>
<br>				<li>ServerLimit:最大接続数</li>
<li>MaxClients:最大プロセス数</li>
<br>			</ul>
<p>クライアントからの接続は、Apacheの親プロセスが待ち受ける<br>→それ以降の処理や対応は、子プロセスが受け付ける<br><br>1.3.xまでは「１接続＝１プロセス」だったらしいが、現在ではマルチスレッドのため、<br>複数の接続を１つのプロセスで処理しているらしい。<br><br>「接続してくるクライアントが増えれば、プロセスが増え、メモリやCPUの負荷も増える」　そりゃそうだ。<br>「”プロセスを増やす”のにも、負荷がかかる」　うん。<br><br>だから、待機プロセスという需要が現れるのかね。<br>接続に備えて、プロセスを起動しておき、待機している状態が待機プロセス。<br><br>MinSpareServersの数だけ、Apache起動後は常にこの数のプロセスが待機している。<br>MaxSpareServersで指定された数以上の待機プロセスにはならない。<br><br>注意書きとしてよく書かれているのが、「クライアントと接続している状態は待機プロセスとは呼ばない」らしい。<br>つまり、待機プロセスから外れたプロセスの分を補充するため、また待機プロセスが作られる事になる。<br>接続が切れて待機プロセスに戻ってきた場合、その時点で補充された待機プロセスと合計してMaxSpareServersの値を超えるようなら、<br>そのプロセスは死ぬ。<br><br>最終的に、MaxClients+MaxSpareServers+親プロセス=起動しているhttpdプロセスとなる…のか？<br><br><br>とりあえずここまで。</p>
</div>