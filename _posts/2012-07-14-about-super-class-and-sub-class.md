---
ID: 153
post_title: >
  クラス(スーパークラス・サブクラス）
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2012/07/about-super-class-and-sub-class/
published: true
post_date: 2012-07-14 14:00:00
---
<br>
<div class="section">
<p>基本だけどイメージがよくできないって人いるんじゃないだろうか？<br><br>よく、車が<span style="color:#00FF33;" class="deco">スーパークラス</span>、スポーツカーやトラックが<span style="color:#00FF33;" class="deco">サブクラス</span>って<br>言うじゃん？　あれ会話や言葉で聞くと頭の悪い俺は<br>「…は？」となるわけで。<br><br>図で表すとこんな感じ。<br><a href="http://f.hatena.ne.jp/killinsun/20120901232330" class="hatena-fotolife" target="_blank" rel="noopener noreferrer"><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20120901/20120901232330.png" alt="f:id:killinsun:20120901232330p:image" title="f:id:killinsun:20120901232330p:image" class="hatena-fotolife"></a><br>このスーパーカーやらトラックにも、<br>例えばトラックの中だったら<b>軽トラ</b>だったり<b>バン型</b>だったり<b>ダンプ</b>だったり<br>いろいろあるわけだ。<br><br><font size="4">そもそもなんでクラス分けなんかするんだよ</font><br>って言われた事があります。<br>プログラミングって非常に面倒くさいものでして、多くの開発者は使い回ししたがります。<br><br>車は、いろんなステータスがあります。<br>ステータスっていうのは、この車で言えば<br></p>
<blockquote>
<p><li>名前</li><br><li>最高速度</li><br><li>燃費</li><br><li>乗車可能な人数</li></p>
</blockquote><br>
<p>大雑把にあげるとこんな感じ。<br><br>でも<br><b>トラックは車だけど、やっぱ物積むじゃん？　どれくらい積めるか設定したいじゃん？</b><br>とかいう要望が出てくるわけで。<br><br>そのために<br></p>
<blockquote>
<p><li>積載量</li></p>
</blockquote><br>
<p>とかいうステータスを追加するわけです。<br>まぁこれぐらいだったら、元々車クラスに突っ込んでおけば問題ないわけだけど、<br><span style="font-size:large;" class="deco">車に対してのサブクラスはいっぱいある</span>わけで、それぞれのサブクラスの要望を満たした親クラスなんて<br><span style="color:#FF0000;font-weight:bold;font-size:x-large;" class="deco">到底作ってられないわけですよ。</span><br><br>そういうわけで、クラスという枠組みを作り、<br><br><span style="font-size:x-large;" class="deco">「じゃあどんな物にも共通する部分だけ作っておいて、その物にしかない特殊なステータスだけサブクラスにぶっ込めばいいんじゃね？」　</span><br>っていうのがオブジェクト指向の考え方。<br><br><span style="color:#FF0000;" class="deco">親クラスに設定したステータスは、子クラスでも使える。</span><br>なのでいちいちトラックやスーパーカーに<b>最大速度</b>なんてステータスを設定しなくても、使えるわけです。<br><br><br><br><pre><br>車クラスの宣言<br>Public Class Car{  <span style="color:#339933;" class="deco">//車クラスの定義</span><br>    int name; <span style="color:#339933;" class="deco">//車種名</span><br>    int maxSpeed; <span style="color:#339933;" class="deco">//最大速度</span><br>    int nenpi; <span style="color:#339933;" class="deco">//燃費</span><br>    int ninzu; <span style="color:#339933;" class="deco">//乗車可能人数</span><br><br>}<br></pre><br><br><pre><br>車クラスのサブクラスである、<b>トラッククラス</b>の宣言<br>Public Class SuperCar extends Car{ <span style="color:#339933;" class="deco">//車クラスからサブクラスを作る際、<span style="color:#FF0000;" class="deco">extends</span>を使ってサブクラス作りますよという宣言をする</span><br>    int sekisai; <span style="color:#339933;" class="deco">//積載量</span><br>}<br></pre><br><br><pre><br>車クラスのサブクラスである、<b>スーパーカークラス</b>の宣言<br>Public Class SuperCar extends Car{ </span><br>    <br>}<br></pre><br>スーパーカークラスなんもねぇwwwwww<br>と思った方、ごめんなさい、スーパーカーに必要なステータスあんま思いつきませんでした。<br>おそらく、メソッド（処理）の話をするときに活きてくると思います。きっと。<br><br>Javaで宣言するとこんな感じですね。<br>Objective-Cの勉強がてら、Objective-Cだとどう宣言するのだろう？</p>
</div>