---
post_title: '【Android App for Kotlin #2】画像を取り扱う(ImageView/ImageButton)'
layout: post
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
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
                                             xmlns:app="http://schemas.android.com/apk/res-auto"
                                             xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
                                             android:layout_height="match_parent">

    <ImageView
            android:id="@+id/imgvIcon"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:layout_marginBottom="8dp"
            android:layout_marginStart="8dp"
            android:layout_marginEnd="8dp"
            app:srcCompat="@mipmap/ic_launcher"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintBottom_toTopOf="@+id/imgbManage"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"  />
    <ImageButton
            android:id="@+id/imgbManage"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginBottom="156dp"
            android:layout_marginStart="8dp"
            android:layout_marginEnd="8dp"
            app:srcCompat="@android:drawable/ic_menu_manage"
            app:layout_constraintBottom_toBottomOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"/>
</android.support.constraint.ConstraintLayout>
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