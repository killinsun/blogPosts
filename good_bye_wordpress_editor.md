まだWordPress5.xのエディタで消耗しているの？

Gutenbergよ、お前とは仲良くなれそうにない

Wordpress5.0にアップグレードしたらとんでもないことになった。
エディタが全く使い物にならなくなってしまった。
こんな効率の悪い状態で技術ブログに時間を費やしたくないので、以前から考えていたVS CodeでMarkdownを書いて、Wordpressにそのまま投稿出来る仕組みを準備した。

## Markdownとは

## 第一段階：Markdownで投稿出来るようにする

### JetPackプラグインのインストール

### ユーザ登録

### 動作確認

## 第二段階：Crayon SyntaxHighlighterを使う(必要な人のみ)

### Crayon SyntaxHighlighterプラグインのインストール

### 特殊文字のエスケープ解除

## 第三段階：VSCode -> Github -> Wordpressで継続的デプロイ環境にする

### 事前準備
- githubでリポジトリを作成

### パーソナルアクセストークン(Personal access tokens)の作成
Settings -> Developper Settings -> Personal access tokensでトークンを作成する。
（Generate Token)から。

- repo 内にある「public_repo」にチェックをつける
生成されたキーをコピーする

### リポジトリにWebhookを設定
リポジトリを開く -> リポジトリのSettings -> Webhooks -> Add webhook
- Payload URLは自分のサイトのURL
- Content typeは　application/jsonを選択
- Secret は自分で好きな文字列をパスワード文字列として指定する。（強固なものにする）
その他はデフォルト


