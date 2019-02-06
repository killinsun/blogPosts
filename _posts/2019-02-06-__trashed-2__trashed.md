---
ID: 444
post_title: '【Android App with Kotlin #5】RecyclerViewを使う'
author: 首無しキリン
post_excerpt: >
  RecyclerViewを一番シンプルかつ実践的な機能で使いました。
layout: post
permalink: https://blog.killinsun.com/?p=444
published: true
post_date: 2019-02-06 15:37:19
---
RecyclerViewを一番シンプルかつ実践的な機能で使いました。

<!--more-->

<h2>作るもの</h2>
<img src="https://blog.killinsun.com/wp-content/uploads/2019/02/05_recyclerview.gif" alt="" width="640" height="400" class="alignnone size-full wp-image-451" />
ImageViewとTextViewがリスト表示され、
タップするとTOASTが通知されるシンプルなものです。

<h2>環境</h2>

このコードは以下の環境で書いています。

<ul>
<li>macOS 10.14.2(Mojave)</li>
<li>Android Studio 3.3 <strong>前回の記事からバージョンップしました</strong></li>
<li>Android SDK 28</li>
<li>gradle 4.6</li>
</ul>

<h2>事前準備</h2>

特にありません。

<h2>レイアウトファイル</h2>

<h3>activity_recycler.xml</h3>

ここにはRecyclerViewのみ置かれます。
RecyclerViewの中身は別のレイアウトファイルで定義します。

<pre><code class="language-xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
                                             xmlns:app="http://schemas.android.com/apk/res-auto" xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
                                             android:layout_height="match_parent"&gt;

    &lt;android.support.v7.widget.RecyclerView
            android:id="@+id/rvAvaterList"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_marginTop="8dp"
            android:layout_marginStart="8dp"
            android:layout_marginEnd="8dp"
            android:layout_marginBottom="8dp"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"
            app:layout_constraintBottom_toBottomOf="parent"/&gt;
&lt;/android.support.constraint.ConstraintLayout&gt;
</code></pre>

<h3>item_profile.xml</h3>

ファイル名はそれっぽい適当な名前です。
RecyclerViewの１要素を表すレイアウトファイルです。
最上位のレイアウト（この場合はConstraintLayout)のheightは
<code>wrap_content</code>もしくは固定値にしないと、１要素が１ページ分のサイズになります。

<pre><code class="language-xml">&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
                                             xmlns:app="http://schemas.android.com/apk/res-auto"
                                             xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
                                             android:layout_height="wrap_content"&gt;

    &lt;LinearLayout
            android:orientation="horizontal"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_constraintTop_toTopOf="parent"
            app:layout_constraintStart_toStartOf="parent"
            app:layout_constraintEnd_toEndOf="parent"&gt;
        &lt;ImageView
                android:id="@+id/ivAvater"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_marginTop="8dp"
                tools:srcCompat="@mipmap/ic_launcher"
                app:layout_constraintStart_toStartOf="parent"
                app:layout_constraintTop_toTopOf="parent"/&gt;
        &lt;TextView
                android:id="@+id/tvAvtName"
                android:layout_width="wrap_content"
                android:layout_height="26dp"
                android:layout_marginTop="8dp"
                android:layout_marginStart="8dp"
                android:layout_marginEnd="8dp"
                app:layout_constraintTop_toTopOf="parent"
                app:layout_constraintStart_toEndOf="@+id/ivAvater"
                app:layout_constraintEnd_toEndOf="parent"
                tools:text="@tools:sample/full_names"/&gt;
    &lt;/LinearLayout&gt;
&lt;/android.support.constraint.ConstraintLayout&gt;
</code></pre>

<h2>ViewHolder</h2>

<h3>AvaterViewHolder.kt</h3>

<code>item_profile</code>で定義したレイアウト内のウィジェットを
後述の<code>Adapter</code>から利用出来る様に宣言しておくファイルになります。
また、<code>インターフェース</code>を用意してありますが、
これはタップした際の動作を定義出来るようにしておく受け口になります。
RecyclerViewの１要素をクラス化しているイメージで捉えています。

<pre><code class="language-java">class AvaterViewHolder(view: View): RecyclerView.ViewHolder(view) {

    val ivAvater: ImageView = view.findViewById(R.id.ivAvater)
    val tvAvtName: TextView = view.findViewById(R.id.tvAvtName)

    /*
    処理を持たないメソッド（インターフェース）を用意する事で
    AdapterやActivityで処理が実装出来るようになる
    */
    interface ItemInterface{
        fun onClickItem(position: Int)
    }
}
</code></pre>

<h2>Adapter</h2>

<h3>AvaterAdapter.kt</h3>

<code>Activity</code>によって生成(インスタンス化)されます。
RecyclerViewの要素数分、行データを読み取り、
<code>ViewHolder</code>とレイアウトファイルを紐づけます。

<pre><code class="language-java">class AvaterAdapter(
        private val avtList:List&lt;String&gt;, 
        private val ItemListener:AvaterViewHolder.ItemInterface
        ): RecyclerView.Adapter&lt;RecyclerView.ViewHolder&gt;(){

    override fun getItemCount(): Int{
        return avtList.size
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {

        val layoutInflater = LayoutInflater.from(parent.context)
        val view: View =  layoutInflater.inflate(R.layout.item_profile, parent, false)

        return AvaterViewHolder(view)
    }

    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        val myHolder:AvaterViewHolder = holder as AvaterViewHolder

        myHolder.tvAvtName.text = avtList[position]
        myHolder.ivAvater.setImageResource(R.mipmap.ic_launcher)

        //テキストをクリックされた時の動作を新規にバインドさせる
        myHolder.tvAvtName.setOnClickListener{
            ItemListener.onClickItem(position)
        }

    }
}
</code></pre>


コードの説明に入ります。

<pre><code class="language-java">class AvaterAdapter(
        private val avtList:List&lt;String&gt;, 
        private val ItemListener:AvaterViewHolder.ItemInterface
        ): RecyclerView.Adapter&lt;RecyclerView.ViewHolder&gt;(){
</code></pre>

第一引数ではActivityから受け取るリスト要素を、
第二引数では、<code>AvaterViewHolder</code>内に宣言したインターフェースを<code>ItemListener</code>という名前で待ちます。

<pre><code class="language-java">    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {

        val layoutInflater = LayoutInflater.from(parent.context)
        val view: View =  layoutInflater.inflate(R.layout.item_profile, parent, false)

        return AvaterViewHolder(view)
    }
</code></pre>

Adapterに与えられたリスト要素（今回は<code>avtList:List&lt;String&gt;</code>)から１要素ずつ受け取り、
viewtypeに応じたRecyclerViewのレイアウトファイルを読み込みます。
最も、今回の場合は全部同じレイアウトのリストなので、<code>viewtype</code>にかかわらず同じ<code>view</code>を読み込みます。

もし、複数種類のレイアウトファイルを使い分ける場合は、<code>getItemViewType</code>を使います。
<code>position</code>(要素の位置)に応じたviewtypeを返却するメソッドです。

サンプル

<pre><code class="language-java">override fun getItemViewType(position:Int):Int{
        val VIEW_T_HEADER:Int = 0
        val VIEW_T_FOOTER:Int = 1
        val VIEW_T_BODY:Int = 3
        when(position){
                //一番最初の要素の場合
                0 -&gt; {
                        return VIEW_T_HEADER
                }
                //リスト要素と同じサイズ + 1 = 最終行の場合
                avtList.size + 1 -&gt; {
                        return VIEW_T_FOOTER
                }
                else -&gt; {
                        return VIEW_T_BODY
                }
        }
}
</code></pre>

<pre><code class="language-java">    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        val myHolder:AvaterViewHolder = holder as AvaterViewHolder

        myHolder.tvAvtName.text = avtList[position]
        myHolder.ivAvater.setImageResource(R.mipmap.ic_launcher)

        //テキストをクリックされた時の動作を新規にバインドさせる
        myHolder.tvAvtName.setOnClickListener{
            ItemListener.onClickItem(position)
        }

    }

</code></pre>

要素毎にテキストや画像をバインド（割り当て）しています。
ここで、<code>setOnCLickListener</code>等のメソッドも普通のウィジェット同様に使えるので、
インターフェース<code>ItemListener</code>が持つメソッド<code>onClickItem</code>を実行させます。（AvaterViewHolderで用意しましたね）

<h2>Activity</h2>

<h3>RecyclerActivity.kt</h3>

Adapterに比べればここは宣言するだけなので非常にシンプルです。

<pre><code class="language-java">class RecyclerActivity: AppCompatActivity(), AvaterViewHolder.ItemInterface {

    val avtList: List&lt;String&gt; = listOf("John","Pawl","Amanda")

    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_recycler)

        rvAvaterList.layoutManager = LinearLayoutManager(this)
        rvAvaterList.adapter = AvaterAdapter(avtList, this)
    }

    override fun onClickItem(position: Int) {
        Toast.makeText(this, "Item Positon:" + position + ", Item:" + avtList[position], Toast.LENGTH_SHORT).show()
    }
}
</code></pre>

<pre><code class="language-java">rvAvaterList.layoutManager = LinearLayoutManager(this)
</code></pre>

<code>activirty_recycler.xml</code>で用意したRecyclerViewのウィジェットをIDから使用しています。
layoutManagerというものを定義しています。
<code>LinearLayoutManager</code>と、<code>GridLayoutManager</code>の２種類があります。
前者はListViewのような１行形式のレイアウトで、
後者は縦横の格子状に要素を持つ事が出来るレイアウトです。
今回は前者を使います。

<pre><code class="language-java">rvAvaterList.adapter = AvaterAdapter(avtList, this)
</code></pre>

先程までで作成したAdapterをこのRecyclerViewに割り当てています。
１つ目の引数では、このアクティビティクラスのメンバ変数として宣言している<code>avtList</code>を、
２つ目の引数ではアクティビティクラスの戻り値として持つ<code>AvaterViewHolder</code>を割り当てています。
（この辺のつながりが自分でもまだよくわかっていません。わかる方いたらコメントやDMで教えてください。)

長くなりましたが、ここまでで一旦、シンプルなRecyclerViewを
扱う事が出来たかと思います。

<h2>リンク</h2>

<h3>次の記事</h3>

<h3>前回の記事</h3>

<a href="https://blog.killinsun.com/?p=431">【Android App with Kotlin #4】ListViewを使う(2)</a>

/以上