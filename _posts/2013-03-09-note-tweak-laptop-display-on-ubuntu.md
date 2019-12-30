---
ID: 168
post_title: >
  Ubuntuだとパソコンのディスプレイが見辛かったので調整メモ
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2013/03/note-tweak-laptop-display-on-ubuntu/
published: true
post_date: 2013-03-09 03:08:17
---
&nbsp;
<div class="section">

Linuxを本格的に使い始めようと思ったは良いものの、
Windows7やMacを使っている時と画面の文字が明るすぎて見えないというか
画面の明るさを調整してもなんとなく見えないというかで困ってた。

調べたらすぐ出てきた。

<a href="https://forums.ubuntulinux.jp/viewtopic.php?id=4263" target="_blank" rel="noopener noreferrer">ディスプレイの調整</a>
<blockquote>xgamma -gamma 1.0</blockquote>
1.0が標準で、0.1単位で指定するといいみたい。
明るすぎたので、今回は0.7ぐらいに設定するとちょうど良かった。

このままだと再起動したときにデフォルトに戻ってしまうので、
<blockquote>~/.profile</blockquote>
こちらに記述しておく。

いい感じいい感じ。
後はバッテリーの消費を抑える方法と、学校へのVPN接続ができるようにしないとなー。

</div>