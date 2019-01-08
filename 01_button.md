---
post_title: 【Android App for Kotlin #1】ボタンウィジェットを使う
layout: post
published: false
---

Kotlinの勉強始めました。　

## レイアウトファイル

app\res\layout\activity_main.xml
デザインタブから「Button」を挿入する。
テキストタブでレイアウトやコードの微修正します。
[a]~[d]ではConstraintLayoutがデフォルトで使用されているので、「このオブジェクトの（上/下/左/右）に配置する」を指定するコードを記述しています。

```xml

<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">

    <TextView
            android:id="@+id/tvHello"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Hello World!"
            app:layout_constraintTop_toTopOf="parent"         //[a]
            app:layout_constraintBottom_toBottomOf="parent"   //[b]
            app:layout_constraintLeft_toLeftOf="parent"       //[c]
            app:layout_constraintRight_toRightOf="parent" />  //[d]
    <Button
            android:id="@+id/btnTouchMe"
            android:text="Touch me!"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginStart="8dp"
            android:layout_marginEnd="8dp"
            android:layout_marginTop="8dp"
            android:layout_marginBottom="8dp"
            app:layout_constraintTop_toBottomOf="@+id/tvHello"  //[a]
            app:layout_constraintBottom_toBottomOf="parent"     //[b]
            app:layout_constraintStart_toStartOf="parent"       //[c]
            app:layout_constraintEnd_toEndOf="parent" />        //[d]

</android.support.constraint.ConstraintLayout>


```

## Activityファイル

app\java\{project}\MainActivity.kt

Buttonに関するコードを記述します。
Buttonをクリックすると、テキストが変わる仕組みです。

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
            tvHello.text = "Good!"
        }
    }
}

```

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

Kotlin Android extentionsが有効であれば、以下のような書き方もできる。

```java

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        btnTouchMe.setOnClickListener{
            tvHello.text = "Good!"
        }
    }
}

```

Javaで書く場合と比べて恐ろしく短い・・・。
