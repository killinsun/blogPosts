---
ID: 353
post_title: '【Android App with Kotlin #1】ボタンウィジェットを使う'
author: 首無しキリン
post_excerpt: ""
layout: post
permalink: >
  https://blog.killinsun.com/2019/01/using-button-with-kotlin-android/
published: true
post_date: 2019-01-08 20:55:16
---
Kotlinの勉強始めました。

<!--more-->

<img class="alignnone size-full wp-image-368" src="https://blog.killinsun.com/wp-content/uploads/2019/01/01_button.gif" alt="" width="640" height="400" />

## 環境

このコードは以下の環境で書いています。

- macOS 10.14.2(Mojave)
- Android Studio 3.2.1
- Android SDK 28
- gradle 4.6

## レイアウトファイル

<img class="alignnone size-full wp-image-369" src="https://blog.killinsun.com/wp-content/uploads/2019/01/baa0800f313393c7986461de9b31c3a3.png" alt="" width="962" height="589" />

デザインタブから「Button」を挿入する。



<img class="alignnone size-full wp-image-370" src="https://blog.killinsun.com/wp-content/uploads/2019/01/69967d1e3cd68512eaefe7b9a3d30167.png" alt="" width="966" height="637" />

テキストタブでレイアウトやコードの微修正します。
```xml
&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;
&lt;android.support.constraint.ConstraintLayout
        xmlns:android=&quot;http://schemas.android.com/apk/res/android&quot;
        xmlns:tools=&quot;http://schemas.android.com/tools&quot;
        xmlns:app=&quot;http://schemas.android.com/apk/res-auto&quot;
        android:layout_width=&quot;match_parent&quot;
        android:layout_height=&quot;match_parent&quot;
        tools:context=&quot;.MainActivity&quot;&gt;

    &lt;TextView
            android:id=&quot;@+id/tvHello&quot;
            android:layout_width=&quot;wrap_content&quot;
            android:layout_height=&quot;wrap_content&quot;
            android:text=&quot;Hello World!&quot;
            app:layout_constraintTop_toTopOf=&quot;parent&quot;         //[a]
            app:layout_constraintBottom_toBottomOf=&quot;parent&quot;   //[b]
            app:layout_constraintLeft_toLeftOf=&quot;parent&quot;       //[c]
            app:layout_constraintRight_toRightOf=&quot;parent&quot; /&gt;  //[d]
    &lt;Button
            android:id=&quot;@+id/btnTouchMe&quot;
            android:text=&quot;Touch me!&quot;
            android:layout_width=&quot;wrap_content&quot;
            android:layout_height=&quot;wrap_content&quot;
            android:layout_marginStart=&quot;8dp&quot;
            android:layout_marginEnd=&quot;8dp&quot;
            android:layout_marginTop=&quot;8dp&quot;
            android:layout_marginBottom=&quot;8dp&quot;
            app:layout_constraintTop_toBottomOf=&quot;@+id/tvHello&quot;  //[a]
            app:layout_constraintBottom_toBottomOf=&quot;parent&quot;     //[b]
            app:layout_constraintStart_toStartOf=&quot;parent&quot;       //[c]
            app:layout_constraintEnd_toEndOf=&quot;parent&quot; /&gt;        //[d]

&lt;/android.support.constraint.ConstraintLayout&gt;
```

`android:id=`は一番上にあるのが個人的に好きなので持ってきました。
[a]~[d]ではConstraintLayoutがデフォルトで使用されているので、「このオブジェクトの（上/下/左/右）に配置する」を指定するコードを記述しています。

## Activityファイル

```java
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)


        //レイアウトファイルで宣言したボタンウィジェットを読み込む [1]
        val touchMeButton: Button = findViewById(R.id.btnTouchMe) as Button
        //レイアウトファイルで宣言したテキストビューを読み込む [2]
        val helloText: TextView = findViewById(R.id.tvHello) as TextView


        //ボタン押下時の処理[3]
        touchMeButton.setOnClickListener{
            tvHello.text = &quot;Good!&quot;
        }
    }
}

```
Buttonに関するコードを記述します。
Buttonをクリックすると、テキストが変わる仕組みです。

### Button [1]、[2]

`Button`クラスを `helloButton`という名前で宣言すると同時に、レイアウトファイルに用意したButtonのIDを引っ張ってきます。
また、[2]で書いているTextViewの内容も同様です。

- `val` は定数。　Javaのfinalと同じ。基本はこっちで宣言するといい。
- `var` は変数。　後々変数の中身を別の値で再代入する場合に利用する。
- `touchMeButton: Button` Button型の定数を`touchMeButon`という名前で用意する
- `as Button` Button型にキャストしています。

### Button [3]

Buttonクラスが持つメソッド`setOnClickListener`を使って、ボタンがタップされた時の動作を記述している。
この時、`touchMeButton`(すなわちButtonクラス）がどんなメソッドを持っているかはここで調べられる。
[public class button](https://developer.android.com/reference/android/widget/Button)

## Activityファイル（別の書き方）

Kotlin Android extentionsが有効であれば、以下のような書き方もできます。

```java
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        btnTouchMe.setOnClickListener{
            tvHello.text = &quot;Good!&quot;
        }
    }
}

```

Javaで書く場合と比べて恐ろしく短い・・・。

## リンク

### 次回の記事
次回はImage ViewとImage Buttonです。
<a href="https://blog.killinsun.com/?p=400">【Android App with Kotlin #2】画像を取り扱う(ImageView/ImageButton)</a>


/以上