---
ID: 179
post_title: >
  python-twitterを使ってTwitterBot開発
author: killinsun
post_excerpt: ""
layout: post
permalink: https://blog.killinsun.com/?p=179
published: true
post_date: 2013-06-06 03:20:28
---
&nbsp;
<div class="section">

環境の構築さえできてしまえば問題無いんだけど、OAuthだったり、
tweetpyだったり、python-twitterだったり、その他いろいろだったり
ライブラリが多すぎてどれを参考にしていいかわからなくなった人向けに。

今回は<span class="deco" style="font-weight: bold;">python-twitter ver0.8.5</span>を使います。
（執筆時点では1.0が出てるけどこちらのほうが解説充実しているため）
<h5>ダウンロード</h5>
&nbsp;
<ul>
 	<li>python-twitter</li>
</ul>
<a href="https://code.google.com/p/python-twitter/" target="_blank" rel="noopener">https://code.google.com/p/python-twitter/</a>
右のDownloadメニューからver 0.8.5をダウンロード。

python-twitterは、他に３種類の依存関係のあるモジュールをインストールしなければならない。
それぞれダウンロードして解凍しておく。
<ul>
 	<li>simplejson</li>
</ul>
<a href="http://cheeseshop.python.org/pypi/simplejson" target="_blank" rel="noopener">http://cheeseshop.python.org/pypi/simplejson</a>
<ul>
 	<li>httplib2</li>
</ul>
<a href="http://code.google.com/p/httplib2/" target="_blank" rel="noopener">http://code.google.com/p/httplib2/</a>
<ul>
 	<li>python-oauth2</li>
</ul>
<a href="http://github.com/simplegeo/python-oauth2" target="_blank" rel="noopener">http://github.com/simplegeo/python-oauth2</a>
<h5>各モジュールのインストール</h5>
&nbsp;

ダウンロードするものを間違えない限り、大体は、解凍したフォルダ内にある
setup.py　を実行することでインストールできる。
管理者権限が必要なので注意。
<pre class="syntax-highlight">sudo pthon setup.py install
</pre>
引数をinstallではなく、buildにすることで、構築と配置を別々の作業に切り分けることもできるらしい。
<h5>OAuth認証のための【鍵】を取得する</h5>
こちらにアクセス
twitter developpers
<a href="https://dev.twitter.com/" target="_blank" rel="noopener">https://dev.twitter.com/</a>

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130607023401" target="_blank" rel="noopener"><img class="hatena-fotolife" title="f:id:killinsun:20130607023401p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130607/20130607023401.png" alt="f:id:killinsun:20130607023401p:image" /></a>
ログインしたら画面右上のアイコンをクリックし、My Applicationsを選択

ここでこれから作成するTwitterアプリケーション(=bot)の登録をする。
<ul>
 	<li>Name</li>
</ul>
アプリケーションの名前。あなたの名前ではありません。
<ul>
 	<li>Description</li>
</ul>
Twitterで外部サービスを使う時に、「このアプリを認証する」という画面を見かけたことがあるかと思います。
そこに出てくる説明文です。
<ul>
 	<li>WebSite</li>
</ul>
もしホームページやブログをお持ちなら
<ul>
 	<li>Callback URL</li>
</ul>
今回は空白でOK


<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130607023404" target="_blank" rel="noopener"><img class="hatena-fotolife" title="f:id:killinsun:20130607023404p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130607/20130607023404.png" alt="f:id:killinsun:20130607023404p:image" /></a>
上手く作成できればこの画面に飛ぶ。
ここがTwitter側で自身のアプリケーションを管理するページである。
ご覧の通り、鍵もここに記載されている。

このままだと、アプリケーションはRead-onlyとなっており、
こちらから行えるのはTLやユーザー情報の取得だけで、ツイートの投稿やフォローなどはできない。

上部にある[settings]タブにあるメニューを見れば、どこを編集すればいいかわかるはず。

<a class="hatena-fotolife" href="http://f.hatena.ne.jp/killinsun/20130607023405" target="_blank" rel="noopener"><img class="hatena-fotolife" title="f:id:killinsun:20130607023405p:image" src="https://cdn-ak.f.st-hatena.com/images/fotolife/k/killinsun/20130607/20130607023405.png" alt="f:id:killinsun:20130607023405p:image" /></a>
Detailsタブに戻り、ページ下部にある
Create my access token　をクリック

これで、
<ul>
 	<li>Consumer Key</li>
 	<li>Consumer Secret</li>
 	<li>Access Token</li>
 	<li>Acccess Secret</li>
</ul>
OAuth認証に必要な４つの鍵が揃った。
これらは外部に漏らさないようにすること。
<h5>指定ユーザーのツイートを取得し、ランダムにつぶやく</h5>
&nbsp;

必要なモジュールをインストールし、
鍵も取得できたところで、いきなりソースコード貼っつけて解説する。
<pre class="syntax-highlight">1 <span class="synComment">#coding:utf-8</span>
2 <span class="synPreProc">import</span> twitter,t_key,random
3
4 consumerKey   =t_key.dict['<span class="synConstant">cons_key</span>']
5 consumerSecret  =t_key.dict['<span class="synConstant">cons_sec</span>']
6 accessToken     =t_key.dict['<span class="synConstant">acc_token</span>']
7 accessSecret  =t_key.dict['<span class="synConstant">acc_sec</span>']
8
9 api = twitter.Api(consumerKey,consumerSecret,accessToken,accessSecret)
10
11 tlAry = api.GetUserTimeline('<span class="synConstant">kill_in_sun</span>',count=100)
12 tweetlist=[]
13
14 <span class="synStatement">for</span> s <span class="synStatement">in</span> tlAry:
15     <span class="synStatement">if</span> s.text[0] != '<span class="synConstant">@</span>':
16         tweetlist.append(s.text)
17
18 api.PostUpdates(status=tweetlist[random.randint(0,99)])
</pre>
&nbsp;
<ul>
 	<li>2行目</li>
</ul>
python-twitterでは、twitterモジュールとしてインポートしている。
<ul>
 	<li>４行目から７行目</li>
</ul>
先ほど取得した認証鍵を予め別ファイルに格納しておき、
インポートしてから格納している。
<ul>
 	<li>9行目</li>
</ul>
注意して貰いたいのは、古い技術ブログに
twitter.Api(username=xxx,password=xxx)といった形の認証方法を記述しているものがあるが、
この認証方式は随分前に廃止になっており、使うことは出来ない。
現在のOAuth認証では、
<pre class="syntax-highlight">api = twitter.Api(consumerKey,consumerSecret,accessToken,accessSecret)
</pre>
このようにして、４つの鍵をパラメータとして渡す必要がある。
<ul>
 	<li>11行目</li>
</ul>
<pre class="syntax-highlight">11 tlAry = api.GetUserTimeline('<span class="synConstant">kill_in_sun</span>',count=100)
</pre>
<ul>
 	<li>GetUserTimeline()</li>
</ul>
このメソッドで指定したユーザーのタイムラインが取得できる。
取得したデータはオブジェクト型として戻り値が返される。
<ul>
 	<li>15行目</li>
</ul>
<pre class="syntax-highlight">15     <span class="synStatement">if</span> s.text[0] != '<span class="synConstant">@</span>':
</pre>
&nbsp;

text　パラメータが、実際の「つぶやいた内容」である。
先頭に[@]が付いているものは公開リプライになってしまうため、取得しないように弾いている。
<ul>
 	<li>16行目</li>
</ul>
<pre class="syntax-highlight">16         tweetlist.append(s.text)
</pre>
今回は、リストを用意し、countで指定した分だけ格納するようにしている。
<ul>
 	<li>18行目</li>
</ul>
&gt;|python|
18 api.PostUpdates(status=tweetlist[random.randint(0,99)])
||&lt;
<ul>
 	<li>PostUpdates()</li>
</ul>
status=[文字列]
ツイートするメソッド。
ご覧のとおり、randomを使い、１００件取得したツイートの中から適当に選び、ツイートしている。
<h5>今後やるべきことメモ</h5>
&nbsp;
<ul>
 	<li>リプライがあった場合に対応</li>
 	<li>一度ツイートしたものは以後、ツイートしないようにする</li>
 	<li>自動フォロー返し</li>
</ul>
</div>