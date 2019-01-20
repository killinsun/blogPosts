---
post_title: '【Android App for Kotlin #4】ListViewを使う(2)'
post_excerpt: >
  ListViewの要素をタップした時の動作を追加しました。
layout: post
published: false
---

ListViewの要素をタップした時の動作を追加しました。

<!--more-->
## 作るもの

前回の記事で作成したListViewに対し、タップイベントを追加しました。

## 環境

このコードは以下の環境で書いています。

- macOS 10.14.2(Mojave)
- Android Studio 3.2.1
- Android SDK 28
- gradle 4.6

## 事前準備

前回の記事の成果物を使います。

## レイアウトファイル

前回の記事で利用したものを使います。

## Activityファイル

```Java
class ListviewActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.listview_activity)

        val heroes:Array<String> = arrayOf("Iron Man", "Captain America", "Thor")
        val realName:Array<String> = arrayOf("Tony Stark", "Steve Rogers", "Thor Odinson") //追加[1]

        val adapter = ArrayAdapter<String>(this,android.R.layout.simple_list_item_1, heroes)
        val lvHeroes: ListView = findViewById(R.id.lvHeroes)

        lvHeroes.adapter = adapter

        /*
            追加[2]
            引数は「parent」、「view」、「position」、「id」の４つで、
            使用しない引数については「_(アンダースコア)」で省略出来る。
        */
        lvHeroes.setOnItemClickListener{ _, _, position, _ ->
            //タップされたアイテムの位置(Position)をインデックスに配列の中身をToastで表示
            Toast.makeText(this,realName[position],Toast.LENGTH_SHORT).show()
        }
    }
}
```

コードの説明に入ります。

### [1]

```java
val realName:Array<String> = arrayOf("Tony Stark", "Steve Rogers", "Thor Odinson") //追加
```

新たに配列を宣言しました。　本来ならばHashMapを用いて`heroes`の要素と一致した
オブジェクトを作るのが望ましいですが、今回は単純にしました。

### [2]

```java
lvHeroes.setOnItemClickListener{ _, _, position, _ ->
}
```

`setOnLitemClickListener`を使って、ListView内の単要素をタップされた際の
アクションイベントを定義しています。

コメントの通り、引数は４つ指定する必要がありますが、Kotlinの場合は`_`で省略出来ます。
今回は`position`という、**どの要素がタップされたか**を判断する為に使う引数を
使用しています。


## リンク

### 次の記事

RecyclerViewについて書きます。

### 前回の記事


/以上