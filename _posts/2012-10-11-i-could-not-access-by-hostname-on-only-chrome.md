---
ID: 181
post_title: >
  Chromeだけホスト名でアクセスできなかった　の話
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2012/10/i-could-not-access-by-hostname-on-only-chrome/
published: true
post_date: 2012-10-11 00:00:00
---
<br>
<div class="section">
<p>自宅内でDNSサーバーを動かし、いくつかあるWEBサーバー（zabbixだったりtomcatだったりOracleのapexだったり）にアクセスしに行くとき、なぜかChromeだけホスト名でアクセスしても<br>名前解決してくれない現象に一ヶ月近く悩まされた。<br>（半分くらい保留してたけど）<br></p>
<h5>問題の切り分け</h5><br>
<p>◯Chrome<br>・「IPアドレス直打ち」で、自宅内のマシンにアクセスはできた<br>・外部のWEBページは問題なく閲覧できる<br>・ホスト名入力しての名前解決ができない<br><br>◯FireFox&IE&Safari<br>・全てにおいて問題なく閲覧できる<br><br>◯その他<br>・dig/nslookupでの名前解決はできる<br>・LAN内において起きる（そもそもLAN内でない限り自宅DNSで名前解決はしない）<br>・Windows,MacOSX,Ubuntu(Chromium)、全てのChromeで起きる。<br>（全てのChromeは設定を同期している）<br><br>Chrome固有の問題で、設定情報を同期しているせいか、全てのマシンからアクセスできませんでした。<br></p>
<h5>検証１：Chromeに格納されている閲覧データなどのキャッシュを削除する</h5><br>
<p>意外とこれで解決する問題だと思うのですが、今回のケース、これだけでは解決しませんでした。<br></p>
<h5>検証２：ChromeとGoogleの同期を解除し、設定も何もかも全てクリアな状態で接続する</h5><br>
<p>これについてはさすがに接続が確認できました。<br>つまり、設定か同期の問題。<br></p>
<h5>検証３：設定だけデフォルトで他のデータを同期させて接続する</h5><br>
<p>ダメでした<br><br></p>
<h5>じゃあ何が原因なの</h5><br>
<p>DNSのキャッシュだけはどうも「設定」からはクリアできないようで、<br>その設定は　アドレスバーに「chrome://net-internals/#dns」と打ち込む事でクリアできるようです。<br>DNSの古いキャッシュが邪魔していました。<br>ここらへんについては検証１ですでにクリアできていたものと思っていたので、見落としていましたね…。<br><br>DNSルックアップエラーだったり、ChromeってDNS周りが意外と面倒な気がする…。</p>
</div>