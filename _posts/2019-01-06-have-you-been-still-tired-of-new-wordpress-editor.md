---
ID: 328
post_title: >
  まだWordPress5.xのエディタで消耗しているの？
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2019/01/have-you-been-still-tired-of-new-wordpress-editor/
published: true
post_date: 2019-01-06 14:36:18
---
Gutenbergよ、お前とは仲良くなれそうにない

Wordpress5.0にアップグレードしたらとんでもないことになった。
エディタが全く使い物にならなくなってしまった。
こんな効率の悪い状態で技術ブログに時間を費やしたくないので、以前から考えていたVS CodeでMarkdownを書いて、Wordpressにそのまま投稿出来る仕組みを準備した。
<!--more-->

## <a id="1">第一段階：Markdownで投稿出来るようにする</a>

### JetPackプラグインのインストール

<img class="alignnone size-full wp-image-334" src="https://blog.killinsun.com/wp-content/uploads/2019/01/79dd5ec551ac7de7c2219e11829810d5.png" alt="" width="500" height="356" />

JetPackプラグインっていろいろあるけど、とりあえずこいつを入れておけばいいです。
やりたいことはMarkdownで投稿出来るようにする事だけど、
他に色々機能がついてきて速度低下がちょっと気になる。この辺りは顕著に見られたらチューニングしていく。

### ユーザ登録

ちなみに入れただけでは使えません。
Wordpressのアカウントを持っていればそれ経由で登録出来るのでやっておきましょう。
そんなに難しい事はなかったはず。

### Markdownの有効化

ダッシュボードから JetPack -&gt; 設定　にあります

<img class="alignnone size-full wp-image-337" src="https://blog.killinsun.com/wp-content/uploads/2019/01/1dc885aaf35551a282e9f23db56348ee.png" alt="" width="700" height="404" />

### 動作確認

以下の文章をコピペで貼り付けてみる

```md
# Header1
## Header 2

**bold**
*Italic*

- a
- b
- c

1. 001
2. 002
3. 003
```

ちなみにエディタは「ビジュアルエディタ」ではなく、「コードエディタ」で貼り付けたほうがうまくいきやすいです。
<img class="alignnone size-full wp-image-340" src="https://blog.killinsun.com/wp-content/uploads/2019/01/8361d7ea3c899cee0bcf825ad40c5162-1.png" alt="" width="700" height="362" />

## <a id="2">第二段階：Crayon SyntaxHighlighterを使う(必要な人のみ)</a>

この状態だと　Markdown書式でソースコードを書いてもスタイリッシュになりません。
元々Crayon SyntaxHighlighterを使っているので、そのまま使い続けます。
(JetPackプラグインとも相性が良いんだってさ)

### Crayon SyntaxHighlighterプラグインのインストール

<img class="alignnone size-full wp-image-341" src="https://blog.killinsun.com/wp-content/uploads/2019/01/6df80f87be69dd47ccc7ba3eded82e1d.png" alt="" width="227" height="166" />
こいつ(Crayon SyntaxHighlighter) を入れてね。

### 特殊文字のエスケープ解除
この状態で

```md
```html

 
&lt;h1&gt;It works!&lt;/h1&gt;
_```

```
テキストを書いても「＜」や「＞」がエスケープされてしまい、正常に表示されない場合があります。

```html
&lt;title&gt; hello world &lt;/title&gt;
&lt;h1&gt; It works!&lt;/h1&gt;
```

そういった場合はCrayon Syntax Highlighterのプラグインにちょっと手を加えて上げる。

#### プラグインの編集方法

WEB管理画面から編集出来ます。
ダッシュボード -&gt; プラグイン -&gt; プラグイン編集

ページ内右上の「編集するプラグインを選択」にて「Crayon SyntaxHighlighter」を選んで「選択」をクリック。

以下、２つのファイルを編集してください。

#### crayon_formatter.class.php

550行目付近の処理をコメントアウトします。

```php
/* Convert &lt;, &gt; and &amp; characters to entities, as these can appear as HTML tags and entities. */
if ($escape) {
//$code = CrayonUtil::htmlspecialchars($code);
}
```

#### langs/default/operator.txt

※フォルダの中に入っているのでたどっていく

以下を削除します

```default
=&amp;
&amp;=
&amp;&amp;
```

## &lt;a id=&quot;3&quot;&gt;第三段階：VSCode -&gt; Github -&gt; Wordpressで継続的デプロイ環境にする&lt;/a&gt;

ここからが本番。　Githubに記事投稿用のリポジトリを用意しておき、masterブランチにpushするとwordpressに自動で投稿してくれる仕組みをセットアップします。

### 事前準備

- githubでリポジトリを作成
- リポジトリに対して何らかのデータ（README.mdがお勧め）を作成しておき、initial commitをしておく。

### パーソナルアクセストークン(Personal access tokens)の作成

Githubにてトークンを生成します。

Settings -&gt; Developper Settings -&gt; Personal access tokensでトークンを作成する。

（Generate Token)から。
&lt;img class=&quot;alignnone size-full wp-image-342&quot; src=&quot;https://blog.killinsun.com/wp-content/uploads/2019/01/d799ed318bc01b494c5638b4cbebbeec.png&quot; alt=&quot;&quot; width=&quot;700&quot; height=&quot;377&quot; /&gt;

- repo 内にある「public_repo」にチェックをつける
生成されたキーをコピーします。次の手順で使用するのでどこかにペーストして覚えておいてください。

&lt;img class=&quot;alignnone size-full wp-image-344&quot; src=&quot;https://blog.killinsun.com/wp-content/uploads/2019/01/82ff3d0eccb6150663f4e0eca6409d08.png&quot; alt=&quot;&quot; width=&quot;700&quot; height=&quot;283&quot; /&gt;

### wordpress-github-sync のインストール

&lt;img class=&quot;alignnone size-full wp-image-345&quot; src=&quot;https://blog.killinsun.com/wp-content/uploads/2019/01/81a0e81cf4c198cfe8dc410720c50cb8.png&quot; alt=&quot;&quot; width=&quot;700&quot; height=&quot;426&quot; /&gt;
こいつをインストール。

### wordpress-github-sync の設定

Wordpressダッシュボード -&gt; 設定 -&gt; Github sync から設定へ。

&lt;img class=&quot;alignnone size-full wp-image-346&quot; src=&quot;https://blog.killinsun.com/wp-content/uploads/2019/01/6eb0190264cad9725c74fa2d38c7751a.png&quot; alt=&quot;&quot; width=&quot;700&quot; height=&quot;349&quot; /&gt;

- Github hostname : 特に変更の必要はなし
- Repository : ユーザ名とリポジトリ名を組み合わせて入力します。　例えば私のユーザ名は「killinsun」で、作成したリポジトリ名が「blogPosts」であれば、　「killinsun/blogPosts」になります。
- Oauth Token: 前述した手順で作成した「パーソナルアクセストークン」です。　先程コピーしておいたものから貼り付けます。
- Webhook Secret : 自分で好きな文字列をパスワードのように指定します。（次の手順で同様のものを入れるので忘れずに）
- Default Import User : Githubリポジトリに投稿された記事をどのWordpress投稿者として作成するか。　選ぶ。
- Webhook callback : 自動で生成されているので、これをコピーして次の手順で使用します。

**次の手順でExport to Githubをする前に必ず「変更を保存」を行っておく**

### リポジトリにWebhookを設定

作成しておいたリポジトリを開く -&gt; リポジトリのSettings -&gt; Webhooks -&gt; Add webhook

&lt;img class=&quot;alignnone size-full wp-image-347&quot; src=&quot;https://blog.killinsun.com/wp-content/uploads/2019/01/ff63a8ae2cdb8f07c33f0de46e38a8e9.png&quot; alt=&quot;&quot; width=&quot;700&quot; height=&quot;604&quot; /&gt;

- 前段の手順で生成された　「Webhook callback」のURLです。
- Content typeは　application/jsonを選択
- 前段の手順で指定した「Webhook Secret」と同じ値です。
その他はデフォルトで問題なし。

### 両方の設定が保存出来たら「Export to Github」

wordpress-github-syncの設定画面で「Export to Github」をクリックすると、既存のWordpress記事の内容がGithubリポジトリにpushされます。
逆にImport from Githubは手動でGithubリポジトリからWordpressに対して同期をかけます。

### 投稿してみる

基本以下のような形で投稿しています。
1. リポジトリをローカル上にクローン
2. リポジトリ直下に「適当な名前.md」で新規Markdownファイルを作成
3. Markdownで記事を書いていく

#### Markdownファイルの先頭に入れる内容

以下のパラメータの挿入が必要です。

```md
---
post_title: 記事タイトル
layout: post
published: false
---

#　見出し
記事内容・・・
```

- post_title: 記事名
- layout: たいていの人は「post」のままでOK.　他に何がサポートされているのかよくわからない。多分[ここ](https://wpdocs.osdn.jp/投稿タイプ#.E3.83.87.E3.83.95.E3.82.A9.E3.83.AB.E3.83.88.E3.81.AE.E6.8A.95.E7.A8.BF.E3.82.BF.E3.82.A4.E3.83.97)見れば幸せになれるはず。
- published: true=で公開　false=下書きのまま

4. コミットしてプッシュする
5. 記事のタグやカテゴリ、画像の挿入はwordpress上で調整

### 修正方法は？

- _drafts に下書き中のMarkdownファイル
- _pagesに固定ページのMarkdownファイル
- _postsに公開中のMarkdownファイル

それぞれ先頭に日付があるのでそれを頼りに編集します。。ただし記事名に日本語が含まれていると正しく表示されないのでその辺りは注意。（記事内の日本語は正しく表現されている）

これらのファイルを編集後に再度コミット・プッシュすることでWordpressにも反映されます。すごい。とても手軽・・・