---
ID: 328
post_title: >
  まだWordPress5.xのエディタで消耗しているの？
author: killinsun
post_excerpt: ""
layout: post
permalink: https://blog.killinsun.com/?p=328
published: false
---
# Gutenbergよ、お前とは仲良くなれそうにない

Wordpress5.0にアップグレードしたらとんでもないことになった。
エディタが全く使い物にならなくなってしまった。
こんな効率の悪い状態で技術ブログに時間を費やしたくないので、以前から考えていたVS CodeでMarkdownを書いて、Wordpressにそのまま投稿出来る仕組みを準備した。

## 第一段階：Markdownで投稿出来るようにする

### JetPackプラグインのインストール

JetPackプラグインっていろいろあるけど、とりあえずこいつを入れておけばいい。
やりたいことはMarkdownで投稿出来るようにする事だけど、
他に色々機能がついてきて速度低下がちょっと気になる。この辺りは顕著に見られたらチューニングしていく。

### ユーザ登録

ちなみに入れただけでは使えません。
Wordpressのアカウントを持っていればそれ経由で登録出来るのでやっておきましょう。
そんなに難しい事はなかったはず。

### Markdownの有効化

ダッシュボードから JetPack -> 設定　にあります

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


## 第二段階：Crayon SyntaxHighlighterを使う(必要な人のみ)

この状態だと　Markdown書式でソースコードを書いてもスタイリッシュになりません。
元々Crayon SyntaxHighlighterを使っているので、そのまま使い続けます。
(JetPackプラグインとも相性が良いんだってさ)

### Crayon SyntaxHighlighterプラグインのインストール

こいつ(Crayon SyntaxHighlighter) を入れてね。


### 特殊文字のエスケープ解除
この状態で

```md

```html

<html>
<head>
<title> hello world </title>
</head>
<body>
<h1> It works! </h1>
</body>
</html>
_```

```
テキストを書いても「＜」や「＞」がエスケープされてしまい、正常に表示されない場合がある。

``` html

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

        /* Convert <, > and & characters to entities, as these can appear as HTML tags and entities. */
        if ($escape) {
            //$code = CrayonUtil::htmlspecialchars($code);
        }

```

#### langs/default/operator.txt

※フォルダの中に入っているのでたどっていく

以下を削除します

```default

=&
&=
&&

```



## 第三段階：VSCode -> Github -> Wordpressで継続的デプロイ環境にする

ここからが本番。　Githubに記事投稿用のリポジトリを用意しておき、masterブランチにpushするとwordpressに自動で投稿してくれる仕組みをセットアップします。

### 事前準備

- githubでリポジトリを作成
- リポジトリに対して何らかのデータ（README.mdがお勧め）を作成しておき、initial commitをしておく。

### パーソナルアクセストークン(Personal access tokens)の作成

Githubにてトークンを生成します。

Settings -> Developper Settings -> Personal access tokensでトークンを作成する。

（Generate Token)から。

- repo 内にある「public_repo」にチェックをつける
生成されたキーをコピーします。次の手順で使用するのでどこかにペーストして覚えておいてください。


### wordpress-github-sync のインストール

こいつをインストール。

### wordpress-github-sync の設定

Wordpressダッシュボード -> 設定 -> Github sync から設定へ。

- Github hostname : 特に変更の必要はなし
- Repository : ユーザ名とリポジトリ名を組み合わせて入力します。　例えば私のユーザ名は「killinsun」で、作成したリポジトリ名が「blogPosts」であれば、　「killinsun/blogPosts」になります。 　
- Oauth Token: 前述した手順で作成した「パーソナルアクセストークン」です。　先程コピーしておいたものから貼り付けます。
- Webhook Secret : 自分で好きな文字列をパスワードのように指定します。（次の手順で同様のものを入れるので忘れずに）
- Default Import User : Githubリポジトリに投稿された記事をどのWordpress投稿者として作成するか。　選ぶ。
- Webhook callback : 自動で生成されているので、これをコピーして次の手順で使用します。

**次の手順でExport to Githubをする前に必ず「変更を保存」を行っておく**


### リポジトリにWebhookを設定

作成しておいたリポジトリを開く -> リポジトリのSettings -> Webhooks -> Add webhook

- 前段の手順で生成された　「Webhook callback」のURLです。
- Content typeは　application/jsonを選択
- 前段の手順で指定した「Webhook Secret」と同じ値です。
その他はデフォルトで問題なし。

### 両方の設定が保存出来たら「Export to Github」

wordpress-github-syncの設定画面で「Export to Github」をクリックすると、既存のWordpress記事の内容がGithubリポジトリにpushされます。
逆にImport from Githubは手動でGithubリポジトリからWordpressに対して同期をかけます。