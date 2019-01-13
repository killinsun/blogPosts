---
ID: 400
post_title: '【Android App for Kotlin #2】画像を取り扱う(ImageView/ImageButton)'
author: 首無しキリン
post_excerpt: >
  ImageViewとImageButtonの使い方をまとめました。
layout: post
permalink: https://blog.killinsun.com/?p=400
published: true
post_date: 2019-01-13 22:54:57
---
ImageViewとImageButtonの使い方をまとめました。

<!--more-->
## 作るもの

<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/02_imgView_Button.gif" alt="" width="640" height="400" class="alignnone size-full wp-image-403" />
Activity内に`ImageView`と`ImageButton`を配置し、`ImageButton`をタップすると
`ImageView`の画像が予め用意した画像に変わります。

## 環境

このコードは以下の環境で書いています。

- macOS 10.14.2(Mojave)
- Android Studio 3.2.1
- Android SDK 28
- gradle 4.6

## 事前準備

app/src/main/res/drawable 内に pngまたはjpgのファイルを格納します。
今回は「いらすとや」さんから適当な画像を拝借してきました。


## レイアウトファイル

<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/e72e013d37ef3effd4ec5d874cf2be67.png" alt="" width="700" height="430" class="alignnone size-full wp-image-406" />
<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/3ef7b7ca71fc7ef590fba1c41f3912f3.png" alt="" width="700" height="441" class="alignnone size-full wp-image-407" />
デザインタブから「Image View」、「ImageButton」を挿入します。

<img src="https://blog.killinsun.com/wp-content/uploads/2019/01/f5d9f66c6b181612c7ed66f2cd044b78.png" alt="" width="700" height="556" class="alignnone size-full wp-image-409" />
ImageViewやImageButton挿入時、どの画像を使用するか選択するウィザードが表示されます。
元々入っている画像もあれば、自分で用意した画像を直ぐに利用することも出来ます。
今回はProjects内の適当な画像を選んでみました。

他にもスパナのアイコンなど、新たに用意する必要のない位デフォルトアイコンの種類が豊富なので、最初に画面上部の検索バーから探してみるのも良いかもしれません。


テキストタブでレイアウトやコードの微修正します。

```xml
&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;android.support.constraint.ConstraintLayout xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;
                                             xmlns:app=&quot;http://schemas.android.com/apk/res-auto&quot;
                                             xmlns:tools=&quot;http://schemas.android.com/tools&quot; android:layout_width=&quot;match_parent&quot;
                                             android:layout_height=&quot;match_parent&quot;&gt;

    &lt;ImageView
            android:id=&quot;@+id/imgvIcon&quot;
            android:layout_width=&quot;wrap_content&quot;
            android:layout_height=&quot;wrap_content&quot;
            android:layout_marginTop=&quot;8dp&quot;
            android:layout_marginBottom=&quot;8dp&quot;
            android:layout_marginStart=&quot;8dp&quot;
            android:layout_marginEnd=&quot;8dp&quot;
            app:srcCompat=&quot;@mipmap/ic_launcher&quot;
            app:layout_constraintTop_toTopOf=&quot;parent&quot;
            app:layout_constraintBottom_toTopOf=&quot;@+id/imgbManage&quot;
            app:layout_constraintStart_toStartOf=&quot;parent&quot;
            app:layout_constraintEnd_toEndOf=&quot;parent&quot;  /&gt;
    &lt;ImageButton
            android:id=&quot;@+id/imgbManage&quot;
            android:layout_width=&quot;wrap_content&quot;
            android:layout_height=&quot;wrap_content&quot;
            android:layout_marginBottom=&quot;156dp&quot;
            android:layout_marginStart=&quot;8dp&quot;
            android:layout_marginEnd=&quot;8dp&quot;
            app:srcCompat=&quot;@android:drawable/ic_menu_manage&quot;
            app:layout_constraintBottom_toBottomOf=&quot;parent&quot;
            app:layout_constraintStart_toStartOf=&quot;parent&quot;
            app:layout_constraintEnd_toEndOf=&quot;parent&quot;/&gt;
&lt;/android.support.constraint.ConstraintLayout&gt;
```

## Activityファイル

```java
class imgActivity : AppCompatActivity(){

    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.imgv_imgb_activity)

        val imgvIcon: ImageView = findViewById(R.id.imgvIcon) //[1]
        val imgbManage: ImageButton = findViewById(R.id.imgbManage) //[2]

        //[3]
        imgbManage.setOnClickListener{
            imgvIcon.setImageResource(R.drawable.pose_kyosyu_boy) //[4]
        }
    }
}
```

Buttonに関するコードを記述します。
Buttonをクリックすると、テキストが変わる仕組みです。

### [1]、[2]

`ImageView`クラス、`Image Button`クラスを それぞれ`imgvIcon`、`imgbManage`という名前で宣言します。
同時に、レイアウトファイルに用意したウィジェットのIDを引っ張ってきます。

- `val` は定数。　Javaのfinalと同じ。基本はこっちで宣言するといい。
- `imgvIcon` ImageView型の定数を`imgvIcon`という名前で用意する
- `imgbManage` ImageButton型の定数を`imgbManage`という名前で用意する

### [3]

ImageButtonクラスが持つメソッド`setOnClickListener`を使って、
ボタンがタップされた時の動作を記述しています。
(厳密にはImageButtonクラスの親クラス、更に親クラスのViewクラスが持つメソッド)

この時、`imgbManage`(すなわちImageButtonクラス）が
どんなメソッドを持っているかはここで調べられます。

[Image Button](https://developer.android.com/reference/android/widget/ImageButton)


### [4]

`setImageResource`メソッドを使って、　drawableディレクトリ内に格納した
画像のファイル名を`R.drawable`というキーから指定しています。
見ての通り、拡張子は指定しません。

## Activityファイル（別の書き方）

Kotlin Android extentionsが有効であれば、以下のような書き方もできます。

valの宣言文がそのまま無くせます。

```java
class imgActivity : AppCompatActivity(){

    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.imgv_imgb_activity)


        imgbManage.setOnClickListener{
            imgvIcon.setImageResource(R.drawable.pose_kyosyu_boy)
        }


    }
}

```

## リンク

### 次の記事
次回はListViewです。

### 前回の記事
<a href="https://blog.killinsun.com/?p=353">【Android App for Kotlin #1】ボタンウィジェットを使う</a>


/以上