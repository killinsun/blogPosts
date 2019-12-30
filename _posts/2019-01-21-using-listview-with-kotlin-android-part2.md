---
ID: 431
post_title: '【Android App with Kotlin #4】ListViewを使う(2)'
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2019/01/using-listview-with-kotlin-android-part2/
published: true
post_date: 2019-01-21 08:01:12
---
ListViewの要素をタップした時の動作を追加しました。

<!--more-->
## 作るもの

<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/04_listview_touchEvent.gif" alt="" width="640" height="400" class="alignnone size-full wp-image-433" />
前回の記事で作成したListViewに対し、タップイベントを追加しました。

## 環境

このコードは以下の環境で書いています。

- macOS 10.14.2(Mojave)
- Android Studio 3.2.1
- Android SDK 28
- gradle 4.6

## 事前準備

<a href="https://blog.killinsun.com/?p=418">前回の記事</a>の成果物を使います。

## レイアウトファイル

<a href="https://blog.killinsun.com/?p=418">前回の記事</a>で利用したものを使います。

## Activityファイル

```Java
class ListviewActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.listview_activity)

        val heroes:Array&lt;String&gt; = arrayOf(&quot;Iron Man&quot;, &quot;Captain America&quot;, &quot;Thor&quot;)
        val realName:Array&lt;String&gt; = arrayOf(&quot;Tony Stark&quot;, &quot;Steve Rogers&quot;, &quot;Thor Odinson&quot;) //追加[1]

        val adapter = ArrayAdapter&lt;String&gt;(this,android.R.layout.simple_list_item_1, heroes)
        val lvHeroes: ListView = findViewById(R.id.lvHeroes)

        lvHeroes.adapter = adapter

        /*
            追加[2]
            引数は「parent」、「view」、「position」、「id」の４つで、
            使用しない引数については「_(アンダースコア)」で省略出来る。
        */
        lvHeroes.setOnItemClickListener{ _, _, position, _ -&gt;
            //タップされたアイテムの位置(Position)をインデックスに配列の中身をToastで表示
            Toast.makeText(this,realName[position],Toast.LENGTH_SHORT).show()
        }
    }
}
```

コードの説明に入ります。

### [1]

```java
val realName:Array&lt;String&gt; = arrayOf(&quot;Tony Stark&quot;, &quot;Steve Rogers&quot;, &quot;Thor Odinson&quot;) //追加
```

新たに配列を宣言しました。　本来ならばHashMapを用いて`heroes`の要素と一致した
オブジェクトを作るのが望ましいですが、今回は単純にしました。

### [2]

```java
lvHeroes.setOnItemClickListener{ _, _, position, _ -&gt;
}
```

`setOnLitemClickListener`を使って、ListView内の単要素をタップされた際の
アクションイベントを定義しています。

コメントの通り、引数は４つ指定する必要がありますが、Kotlinの場合は`_`で省略出来ます。
今回は`position`という、**どの要素がタップされたか**を判断する為に使う引数を
使用しています。


## リンク

### 次の記事

<a href="https://blog.killinsun.com/?p=444">【Android App with Kotlin #5】RecyclerViewを使う</a>

### 前回の記事
<a href="https://blog.killinsun.com/?p=418">【Android App with Kotlin #3】ListViewを使う</a>

/以上