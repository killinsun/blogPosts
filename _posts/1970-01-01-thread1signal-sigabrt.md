---
ID: 155
post_title: Thread1:signal SIGABRT
author: killinsun
post_excerpt: ""
layout: post
permalink: https://blog.killinsun.com/?p=155
published: true
post_date: 1970-01-01 00:00:00
---
<div class="section">
<p>はてなダイアリーも最近なんとか慣れてきました。<br><br><h5>タイマーアプリを作る</h5><br>今回は、こちらのサイトの解説を参考に、iPhoneにてタイマーアプリを作ってました。</p>
<blockquote>
<p><a href="http://d.hatena.ne.jp/moto_maka/20081118/1226953067#03" target="_blank">もとまか日記</a></p>
</blockquote><br>
<p>内容は古いもので、まだXcodeにStoryBoardがなかった時代のもの。<br>一応、InterfaceBuilderでの変数の結合などのやり方はわかっているので、<br>StoryBoardを使って、今回は作成してみる事に。<br>（コードそのものは多分、対してかわらないだろうと思ったため）<br><br><br><h5>原因不明のエラー</h5><br><br>だが、最後の最後にコンパイルして実行してみると、<br>このようなエラー（シグナル）が発生し、シミュレータでは何も動作しない。<br><br><br><a href="http://f.hatena.ne.jp/killinsun/20120904001317" class="hatena-fotolife" target="_blank"><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20120904/20120904001317.png" alt="f:id:killinsun:20120904001317p:image" title="f:id:killinsun:20120904001317p:image" class="hatena-fotolife"></a><br><br><br>シミュレータ内でアプリを終了して再起動させるにも、今度は真っ暗のまま。<br><br>これはどうしたものか…ととりあえず　<span style="color:#66CC99;" class="deco">[Thread 1:signal SIGABRT]</span>でググってみる事に。<br><br><br><h5>情報収集してわかったこと</h5><br>どうも、StoryBoard(IB)内にミスがあると出るらしい。<br><br>例えば、　<br><span style="color:#FF6666;" class="deco">変数A</span>と<span style="color:#6633CC;" class="deco">ラベル</span>をOutletしていたとする。<br>プログラムのコーディングの過程で、<span style="color:#ff6666;" class="deco">変数A</span>を<span style="font-weight:bold;" class="deco">書き換えてしまう。</span><br>すると、StoryBoard(IB)の<span style="color:#6633cc;" class="deco">ラベル</span>は<span style="color:#ff6666;" class="deco">変数A</span>と組んでいるにも関わらず、コード側で変数を書き換えて相手を見失ったため、<br>Outletが解除された状態になる。<br><br>その状態で実行すると発生するシグナルがこの <span style="color:#66cc99;" class="deco">Thread1 :signal SIGABRT</span>らしい。<br><br>というわけで、実際にStoryBoardをみてみることに。<br><br><br><a href="http://f.hatena.ne.jp/killinsun/20120904001318" class="hatena-fotolife" target="_blank"><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20120904/20120904001318.png" alt="f:id:killinsun:20120904001318p:image" title="f:id:killinsun:20120904001318p:image" class="hatena-fotolife"></a><br><br><br>…特にエラーはない…。<br>ヘッダーファイルをみてみる。<br><br><br><br><a href="http://f.hatena.ne.jp/killinsun/20120904001319" class="hatena-fotolife" target="_blank"><img src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20120904/20120904001319.png" alt="f:id:killinsun:20120904001319p:image" title="f:id:killinsun:20120904001319p:image" class="hatena-fotolife"></a><br>左側をみての通り、Outletが成功しているという旨の◎アイコンができている。<br><br><br><pre><br>　　　 　＿＿＿_　　　　━┓<br>　　　／　　 　 　＼　　 ┏┛<br>　 ／　　＼　　 ,＿＼.　 ・<br>／ 　　 （●）゛ （●） ＼<br>|　 ∪　　 （__人__）　 　 |<br>/　　　　 ∩ノ ⊃　　／<br>(　 ＼　／ ＿ノ　|　 |<br>.＼　“　　／＿＿|　 |<br>　　＼ ／＿＿＿ ／<br></pre><pre><br><br>　 　　　＿＿＿_<br>　　　／　　 　 　＼<br>　 ／　　　　 　 　　＼<br>／ 　　　　　　　　　　＼<br>|　 　　　＼　　 ,＿　　 |<br>/　　ｕ　 ∩ノ ⊃―）／<br>(　 ＼　／ ＿ノ　|　 |<br>.＼　“　　／＿＿|　 |<br>　　＼ ／＿＿＿ ／<br></pre><br><h5>仕方ないので更にググる。ググる。ググる。</h5><br></p>
<blockquote>
<ul>
<li>アプリを削除して入れ直すと直るよ！</li>
<li>XCODEを再起動をすると直るよ！</li>
<li>Macを再起動すると直るよ！！！！！！！←効果大らしい</li>
</ul>
</blockquote><br>
<p>というわけで全部やってみたけど　<span style="font-size:x-large;" class="deco">効果なかったわ。</span><br><br>つまり、そうなると　コーディングの部分に問題があるのか…？<br><br><br>XCODE.....ようわからんぞ.....<br><br><br>情報ご存知のいたらコメントなど頂けると幸いです。</p>
</div>