---
ID: 170
post_title: >
  Ubuntu12.10　ノートPCで画面の明るさ調整
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2013/03/brightness-tweak-on-ubuntu-12-10-on-laptop/
published: true
post_date: 2013-03-17 23:24:11
---
&nbsp;
<div class="section">
<h5>症状</h5>
Ubuntu12.10を導入したものの、ノートPCにて
<blockquote>Fn ＋ ← or →</blockquote>
で明るさが調整できない事にハマってたので、設定メモ。

仕様構成
マシン：AcerAspireS3
OS:Ubuntu 12.10

Fnキーを使っても、
設定メニューから明るさを調節するバーをいじっても、
画面では変わってるらしいんだが、実際に変わらない症状です。
<h5>以下、メモ</h5>
端末から
<blockquote>sudo vi /etc/default/grub</blockquote>
grubの設定ファイルを開く。

開いてすぐのこの項目。
<blockquote>GRUB_CMDLINE_LINUX_DEFAULT=""</blockquote>
[""]内に以下を追加
<blockquote>acpi_osi=Linux acpi_backlight=vendor</blockquote>
何をやってるのかはよくわからないけどおまじないということで。
まぁなんとなく察しはつきますね。
<blockquote>GRUB_CMDLINE_LINUX_DEFAULT="acpi_osi=Linux acpi_backlight=vendor"</blockquote>
このようになったら保存して終了。
<blockquote>update-grub</blockquote>
と打ち込み、再起動。

これで、画面の明るさ調整ができるようになった。
ついでに、音量の調整も効くようになった。

</div>