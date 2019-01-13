---
ID: 400
post_title: '【Android App for Kotlin #2】画像を取り扱う(ImageView/ImageButton)'
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: https://blog.killinsun.com/?p=400
published: false
---
ImageViewとImageButtonの使い方をまとめました。

<!--more-->

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

デザインタブから「Image View」、「ImageButton」を挿入します。

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

### Button [1]、[2]

`ImageView`クラス、`Image Button`クラスを それぞれ`imgvIcon`、`imgbManage`という名前で宣言します。
同時に、レイアウトファイルに用意したウィジェットのIDを引っ張ってきます。

- `val` は定数。　Javaのfinalと同じ。基本はこっちで宣言するといい。
- `imgvIcon` ImageView型の定数を`imgvIcon`という名前で用意する
- `imgbManage` ImageButton型の定数を`imgbManage`という名前で用意する

### Button [3]

ImageButtonクラスが持つメソッド`setOnClickListener`を使って、
ボタンがタップされた時の動作を記述しています。
(厳密にはImageButtonクラスの親クラス、更に親クラスのViewクラスが持つメソッド)

この時、`imgbManage`(すなわちImageButtonクラス）が
どんなメソッドを持っているかはここで調べられます。

[public class button](https://developer.android.com/reference/android/widget/Button)

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


次回はListViewです。

/以上