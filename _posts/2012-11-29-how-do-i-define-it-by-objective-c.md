---
ID: 154
post_title: >
  Objective-Cだとどう宣言するんだよ
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2012/11/how-do-i-define-it-by-objective-c/
published: true
post_date: 2012-11-29 00:00:00
---
<div class="section">
<p>ここからはちょっと自分のためのメモ。<br></p>
<blockquote>
<p><a href="http://www.atmarkit.co.jp/fcoding/articles/objc/03/objc03a.html">第3回 Objective-Cのクラス定義を理解しよう</a></p>
</blockquote>
<p>どうもこちらのサイトで解説している情報を読む限り、<br><br>Objective-Cでは<br></p>
<blockquote>
<ul>
<li>すべてのクラス（オブジェクト）のスーパークラスは<b>NSObject</b>であり、どんなクラスもここから継承している。（ちなみにJavaは<b>Object</b>)</li>
<li>Javaと違って、NSObjectから継承している事を宣言しなければならない。</li>
<li>クラスは<b>宣言部</b>と<b>実装部</b>に分かれている</li>
<li>宣言部では<b>@Interface</b> 実装部では<b>@Implements</b>を用いて宣言を行う。</li>
</ul>
</blockquote>
<p>こんな感じだろうか？<br><br>上の車クラスやスーパーカークラスの宣言をObjective-C風に宣言するとこうなる<br><br><pre><br>車クラスの宣言<br><br>@interface Car :NSObject{  <span style="color:#339933;" class="deco">/*車クラスの定義 */</span><br>    int name; <span style="color:#339933;" class="deco">/*車種名 */</span><br>    int maxSpeed; <span style="color:#339933;" class="deco">/*最大速度 */</span><br>    int nenpi; <span style="color:#339933;" class="deco">/*燃費 */</span><br>    int ninzu; <span style="color:#339933;" class="deco">/*乗車可能人数 */</span><br>}<br>@end<br><br>@implements Car :NSObject<br>@end<br><br></pre><br><br>…こ、こんな感じなのだろうか？<br>宣言部と、実装部の違いがまだよくわかっていないなー…。<br><br>もう少し、掘り下げてやっていきたいが、そろそろ時間も時間なので<br>明日にするとします。</p>
</div>