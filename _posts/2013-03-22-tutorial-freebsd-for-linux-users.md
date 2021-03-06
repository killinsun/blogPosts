---
ID: 171
post_title: LinuxユーザーのFreeBSD入門メモ
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2013/03/tutorial-freebsd-for-linux-users/
published: true
post_date: 2013-03-22 01:32:16
---
&nbsp;
<div class="section">

UbuntuとCentOSしか触った事なかったのでメモ。
トラブル☆しゅーたーず#5で知り合い、懇親会でかなりお世話になった
@usaturn様のご好意で
HPの激安サーバー「ML115G5」を実験用に譲って頂いたので、
早速普段触らないFreeBSDをインストールしてみた。

スペックとかはまた次回の記事で。

右も左もわからないので、
インストール後から思いついた事をやってみる。
<h5>インストール直後</h5>
&nbsp;
<blockquote>#adduser YourName</blockquote>
&nbsp;

でアカウント作成

デフォルトのシェルがshになってるので
とりあえず cshに変更。
<h5>Portsとは</h5>
FreeBSDでは、パッケージ管理？の方法にportsというものを使っているらしい。

予めBSDのインストール時にアプリケーションなどの情報だけを格納しておき、
ユーザーが後から必要なものだけインストールする方式らしい。
<blockquote>/usr/ports/</blockquote>
&nbsp;

にいろいろ入ってる。
カテゴライズされてた。

だが、インストールされてなかったので、入れる。
<blockquote># fetch <a href="ftp://ftp.freebsd.org/pub/FreeBSD/ports/ports/ports.tar.gz" target="_blank" rel="noopener noreferrer">ftp://ftp.freebsd.org/pub/FreeBSD/ports/ports/ports.tar.gz</a>
# tar -zxvf ./ports.tar.gz -C /usr/</blockquote>
&nbsp;
<h5>アップデート</h5>
&nbsp;
<blockquote>#portsnap fetch
#portsnap extract
#portsnap update</blockquote>
&nbsp;
<h5>ためしにVimを入れる</h5>
&nbsp;
<blockquote>#cd /usr/ports/editors/vim
#make install</blockquote>
&nbsp;

これでvimをとりあえず入れてみた。
<h5>FreeBSDにBashを入れる</h5>
<blockquote>#cd /usr/ports/shells/bash
#make install</blockquote>
&nbsp;

インストール時はports関係を入れてなかったので、
依存関係のプログラムをインストールする関係で、
vimとbashの２つを入れるだけでもかなり時間を食った。
<blockquote>#chsh -s /usr/local/bin/bash</blockquote>
&nbsp;

デフォルトのシェルをbashに設定。
<h5>一応サーバーとして動かすので、IPアドレスの固定をする</h5>
IPアドレスの固定したいけど
<blockquote>/etc</blockquote>
&nbsp;

の中によくある
<blockquote>/sysconfig/</blockquote>
が無いので
ネットワーク関連の設定はどこですればいいのやら。
<blockquote>/etc/rc.conf</blockquote>
&nbsp;

で設定するみたい。

BSDの場合は、Linuxであるような
eth0~xではなく、様々な表記らしいので、ifconfigで確認
<blockquote>$ ifconfig
<span class="deco" style="color: #ff0000;">bge0</span>: flags=8843&lt;UP,BROADCAST,RUNNING,SIMPLEX,MULTICAST&gt; metric 0 mtu 1500
options=c019b&lt;RXCSUM,TXCSUM,VLAN_MTU,VLAN_HWTAGGING,VLAN_HWCSUM,TSO4,VLAN_HWTSO,LINKSTATE&gt;
ether d8:d3:85:ae:0a:f2
inet 192.168.0.7 netmask 0xffffff00 broadcast 192.168.0.255
nd6 options=29&lt;PERFORMNUD,IFDISABLED,AUTO_LINKLOCAL&gt;
media: Ethernet autoselect (100baseTX &lt;full-duplex&gt;)
status: active
lo0: 略</blockquote>
&nbsp;

bge0ね。　なるほど。
ところでなんでbgeなんだろう？
eth0はEthernetだってわかったけど。
<blockquote>#vim /etc/rc.conf</blockquote>
&nbsp;

下記を追加
<blockquote>network_interfaces="bge0"
ifconfig_bge0="inet 192.168.0.127 netmask 255.255.255.0 broadcast 192.168.0.255"</blockquote>
&nbsp;
<blockquote>#service network restart</blockquote>
&nbsp;

とやりたいところだが、　<span class="deco" style="color: #ff0000;">network</span>ではなく　<span class="deco" style="color: #ff0000;">netif</span>らしい
<blockquote>#service netif restart</blockquote>
&nbsp;

これで設定完了。

</div>