---
ID: 418
post_title: '【Android App with Kotlin #3】ListViewを使う'
author: 首無しキリン
post_excerpt: >
  まずはListViewに適当なデータを表示させるシンプルなものを用意しました。
layout: post
permalink: >
  https://blog.killinsun.com/2019/01/using-listview-with-kotlin-android-part1/
published: true
post_date: 2019-01-15 23:15:56
---
まずはListViewに適当なデータを表示させるシンプルなものを用意しました。

<!--more-->
## 作るもの

<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/03_listview.gif" alt="" width="640" height="400" class="alignnone size-full wp-image-421" />
ListviewActivity.kt内で宣言したString型のコレクションを表示させるListViewです。

## 環境

このコードは以下の環境で書いています。

- macOS 10.14.2(Mojave)
- Android Studio 3.2.1
- Android SDK 28
- gradle 4.6

## 事前準備

特にありません。
表示させるアイテムのネタを考えておきましょう。

## レイアウトファイル

<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/3c645c95809a7bda6124f034e0b1eacf.png" alt="" width="1023" height="632" class="alignnone size-full wp-image-420" />
デザインタブから「ListView」を挿入します。
Paletteの`Legacy`に入っているので注意しましょう。


テキストタブでレイアウトやコードの微修正します。

```xml
&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;android.support.constraint.ConstraintLayout xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;
                                             xmlns:app=&quot;http://schemas.android.com/apk/res-auto&quot; xmlns:tools=&quot;http://schemas.android.com/tools&quot; android:layout_width=&quot;match_parent&quot;
                                             android:layout_height=&quot;match_parent&quot;&gt;

    &lt;ListView
            android:id=&quot;@+id/lvHeroes&quot;
            android:layout_width=&quot;368dp&quot;
            android:layout_height=&quot;495dp&quot;
            android:layout_marginTop=&quot;8dp&quot;
            android:layout_marginBottom=&quot;8dp&quot;
            android:layout_marginStart=&quot;8dp&quot;
            android:layout_marginEnd=&quot;8dp&quot;
            app:layout_constraintTop_toTopOf=&quot;parent&quot;
            app:layout_constraintBottom_toBottomOf=&quot;parent&quot;
            app:layout_constraintStart_toStartOf=&quot;parent&quot;
            app:layout_constraintEnd_toEndOf=&quot;parent&quot; /&gt;
&lt;/android.support.constraint.ConstraintLayout&gt;
```

## Activityファイル

```Java
class ListviewActivity: AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.listview_activity)

        val heroes:Array&lt;String&gt; = arrayOf(&quot;Iron Man&quot;, &quot;Captain America&quot;, &quot;Thor&quot;) //[1]
        val adapter = ArrayAdapter&lt;String&gt;(this,android.R.layout.simple_list_item_1, heroes) //[2]
        val lvHeroes: ListView = findViewById(R.id.lvHeroes) //[3]

        lvHeroes.adapter = adapter //[4]

    }
}
```

コードの説明に入ります。

### [1]

String型が格納されるコレクション(配列)を宣言すると同時に値を代入しています。

### [2]

ListViewというレイアウトと、`[1]`で宣言したコレクションを紐付けるアダプターです。

- `this`
    コンテキストです。まだ自分もよくわかっていないので調べて加筆します。
- `android.R.layout.simple_list_item_1`
    はじめから用意されているListViewの項目１つ分のレイアウトです。
    独自のレイアウトを適用させる場合は、この引数を変える事になります。
- `heroes`
    `[1]`で宣言したコレクションの名前で、アダプターにセットしています。

### [3]

レイアウトファイルで用意した`ListView`をActivityファイルで使えるように宣言しています。
Kotlin Andorid extentionsが有効であれば、この項目は書かなくてもレイアウトファイルで
宣言した`android:id`でそのまま扱えます。

### [4]

`[3]`で宣言した`ListView`と、`[2]`で宣言したadapterを紐づけています。

## リンク

### 次の記事

次回はListViewで用意したアイテムにタッチすると
処理が実行されるようにします。
<a href="https://blog.killinsun.com/?p=431">【Android App for Kotlin #4】ListViewを使う(2)</a>

### 前回の記事
<a href="https://blog.killinsun.com/?p=400">【Android App with Kotlin #2】画像を取り扱う(ImageView/ImageButton)</a>


/以上