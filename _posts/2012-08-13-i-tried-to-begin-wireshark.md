---
ID: 148
post_title: WireShark入門してみました
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2012/08/i-tried-to-begin-wireshark/
published: true
post_date: 2012-08-13 23:01:50
---
<div class="section">
<p>セキュリティキャンパス２０１２っていうのに応募していたんだけど<br>その応募期間の時にTwitterで<br>「もし受かったとしたら、それまでの間にできる事って何だろう。今からできる予習ってなんだろう」<br>ってつぶやいたら講師の方が<br>「Wiresharkでパケットひたすら眺めとけ」とおっしゃっていたので、<br>早速導入して我がネットワークのパケットをキャプチャしていたんですが<br>全く使い方がわからずに断念して一時保留・・・。<br><br>結局、セキュリティキャンパス自体は落選したものの、<br>セキュリティやネットワークの勉強は続けるべきだと考えているので<br>改めて書籍を使って勉強することに。</p>
<div class="hatena-asin-detail">
<a href="http://www.amazon.co.jp/dp/4897978750/?tag=hatena_st1-22&ascsubtag=d-1ajs09"><img src="https://images-fe.ssl-images-amazon.com/images/I/513SNbjYFgL._SL160_.jpg" class="hatena-asin-detail-image" alt="パケットキャプチャ入門　改訂版 (ＬＡＮアナライザＷｉｒｅｓｈａｒｋ活用術)" title="パケットキャプチャ入門　改訂版 (ＬＡＮアナライザＷｉｒｅｓｈａｒｋ活用術)"></a>
<div class="hatena-asin-detail-info">
<p class="hatena-asin-detail-title"><a href="http://www.amazon.co.jp/dp/4897978750/?tag=hatena_st1-22&ascsubtag=d-1ajs09">パケットキャプチャ入門　改訂版 (ＬＡＮアナライザＷｉｒｅｓｈａｒｋ活用術)</a></p>
<ul>
<li><span class="hatena-asin-detail-label">作者:</span> <a href="http://d.hatena.ne.jp/keyword/%C3%DD%B2%BC%A1%A1%B7%C3" class="keyword">竹下　恵</a></li>
<li><span class="hatena-asin-detail-label">出版社/メーカー:</span> <a href="http://d.hatena.ne.jp/keyword/%A5%EA%A5%C3%A5%AF%A5%C6%A5%EC%A5%B3%A5%E0" class="keyword">リックテレコム</a></li>
<li><span class="hatena-asin-detail-label">発売日:</span> 2011/05/27</li>
<li><span class="hatena-asin-detail-label">メディア:</span> 大型本</li>
<li><span class="hatena-asin-detail-label">購入</span>: 4人 <span class="hatena-asin-detail-label">クリック</span>: 62回</li>
<li><a href="http://d.hatena.ne.jp/asin/4897978750" target="_blank" rel="noopener noreferrer">この商品を含むブログ (5件) を見る</a></li>
</ul>
</div>
<div class="hatena-asin-detail-foot"></div>
</div>
<p><br><br>中身は意外と読みやすくなってます。<br>基本の基本の操作（キャプチャしたパケットの保存とか）とかから解説進めてます。<br><br>３章あたりで実際にパケットをキャプチャし、<br>フィルタリングの説明などがあり、<br>４章でプロトコルやレイヤの説明。<br><br>オライリージャパンの</p>
<div class="hatena-asin-detail">
<a href="http://www.amazon.co.jp/dp/4873115140/?tag=hatena_st1-22&ascsubtag=d-1ajs09"><img src="https://images-fe.ssl-images-amazon.com/images/I/519jXXwi5cL._SL160_.jpg" class="hatena-asin-detail-image" alt="Hacking: 美しき策謀 第2版 ―脆弱性攻撃の理論と実際" title="Hacking: 美しき策謀 第2版 ―脆弱性攻撃の理論と実際"></a>
<div class="hatena-asin-detail-info">
<p class="hatena-asin-detail-title"><a href="http://www.amazon.co.jp/dp/4873115140/?tag=hatena_st1-22&ascsubtag=d-1ajs09">Hacking: 美しき策謀 第2版 ―脆弱性攻撃の理論と実際</a></p>
<ul>
<li><span class="hatena-asin-detail-label">作者:</span> <a href="http://d.hatena.ne.jp/keyword/Jon%20Erickson" class="keyword">Jon Erickson</a>,<a href="http://d.hatena.ne.jp/keyword/%C2%BC%BE%E5%B2%ED%BE%CF" class="keyword">村上雅章</a></li>
<li><span class="hatena-asin-detail-label">出版社/メーカー:</span> <a href="http://d.hatena.ne.jp/keyword/%A5%AA%A5%E9%A5%A4%A5%EA%A1%BC%A5%B8%A5%E3%A5%D1%A5%F3" class="keyword">オライリージャパン</a></li>
<li><span class="hatena-asin-detail-label">発売日:</span> 2011/10/22</li>
<li><span class="hatena-asin-detail-label">メディア:</span> 単行本（ソフトカバー）</li>
<li><span class="hatena-asin-detail-label">購入</span>: 9人 <span class="hatena-asin-detail-label">クリック</span>: 163回</li>
<li><a href="http://d.hatena.ne.jp/asin/4873115140" target="_blank" rel="noopener noreferrer">この商品を含むブログ (19件) を見る</a></li>
</ul>
</div>
<div class="hatena-asin-detail-foot"></div>
</div>
<p>この書籍も一応先月に買って読んでるですが<br>こっちはひたすら文章って感じで、説明は丁寧なんだけど、長く読み続けるのは苦痛。<br>（でも間あけるとわからなくなるというジレンマ）<br><br><br>それに比べると、サイズもちょこっと大き目な分、読みやすく構成されてるなと思いました。<br>（図解入りだし）<br><br>とりあえず、名前解決のやり方がわかり、実際にhttpプロトコルを重点的にキャプチャして<br>構造をのぞいてみたり…っていうのが現状です。<br>いろいろ試してみる価値はあるかな。</p>
</div>