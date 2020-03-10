# Material Design Grid Card

[https://github.com/material-components/material-components-android-codelabs/archive/102-starter.zip](https://github.com/material-components/material-components-android-codelabs/archive/102-starter.zip)
# Android界面
### 添加项目依赖
```kotlin
dependencies {
    api 'com.google.android.material:material:1.1.0-alpha06'
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.google.code.gson:gson:2.8.5'
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:1.3.21"
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test:core:1.1.0'
    androidTestImplementation 'androidx.test.ext:junit:1.1.0'
    androidTestImplementation 'androidx.test:runner:1.2.0-alpha05'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0-alpha05'
}
```

初步效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582701363855-846d7f05-ae3b-43c6-8cbe-9655ff74fcdd.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=69415&status=done&style=none&width=405)

登录成功后还未给成功界面添加内容，下一步将为登陆成功后的界面添加一些功能。

### 添加顶部导工具栏
修改shr_product_grid_fragment.xml代码文件，使用以下代码替代原本的`<LinearLayout>`：
```kotlin
<com.google.android.material.appbar.AppBarLayout
   android:layout_width="match_parent"
   android:layout_height="wrap_content">

   <androidx.appcompat.widget.Toolbar
       android:id="@+id/app_bar"
       style="@style/Widget.Shrine.Toolbar"
       android:layout_width="match_parent"
       android:layout_height="?attr/actionBarSize"
	   android:background="@color/colorPrimary"
       app:title="@string/shr_app_name" />
</com.google.android.material.appbar.AppBarLayout>
```

#### 添加图标
在上面添加的Toolbar中，仅仅实现了单一的文本显示，为了使Toolbar内容更加丰富，需要添加一个导航图标。
图标的代码如下：
```kotlin
app:navigationIcon="@drawable/shr_menu"
```

修改后的xml文件内容如下：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
   xmlns:app="http://schemas.android.com/apk/res-auto"
   xmlns:tools="http://schemas.android.com/tools"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   tools:context=".ProductGridFragment">
  
   <com.google.android.material.appbar.AppBarLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content">

       <androidx.appcompat.widget.Toolbar
           android:id="@+id/app_bar"
           style="@style/Widget.Shrine.Toolbar"
           android:layout_width="match_parent"
           android:layout_height="?attr/actionBarSize"
		   android:background="@color/colorPrimary"
           app:navigationIcon="@drawable/shr_menu"
           app:title="@string/shr_app_name" />
   </com.google.android.material.appbar.AppBarLayout>
</FrameLayout>
```

拓：也可以在Toolbar的末端添加按钮，官方称之为[操作按钮](https://developer.android.com/training/appbar/actions)。

#### 设置ActionBar
以上代码为Fragment内添加了一个Bar，还需通过setSupportActionBar将Toolbar作为ActionBar使用。

修改ProductGridFragment.kt：
```kotlin
override fun onCreateView(
       inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
   // Inflate the layout for this fragment with the ProductGrid theme
   val view = inflater.inflate(R.layout.shr_product_grid_fragment, container, false)

   // Set up the toolbar.
   (activity as AppCompatActivity).setSupportActionBar(view.app_bar)

   return view;
}
```

若不如此设置，ToolBar应该是无法作为一个正常的ActionBar进行事件响应的。

#### 重写onCreateOptionsMenu
```kotlin
override fun onCreateOptionsMenu(menu: Menu, menuInflater: MenuInflater) {
   menuInflater.inflate(R.menu.shr_toolbar_menu, menu)
   super.onCreateOptionsMenu(menu, menuInflater)
}
```
这个操作的目的是为了将shr_toolbar_menu.xml的内容加入到toolbar。

#### 重写onCreate
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
   super.onCreate(savedInstanceState)
   setHasOptionsMenu(true)
}
```
setHasOptionsMenu默认属性为false，设置为true则可以允许设置自定义OptionMenu，否则onCreateOptionsMenu的代码将不起作用。

展示效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582703184266-62860181-a6af-4b0a-bc27-a79140432087.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=51942&status=done&style=none&width=405)


### 添加Card
为shr_product_grid_fragment.xml布局文件内添加如下代码：
```kotlin
<com.google.android.material.card.MaterialCardView
   android:layout_width="160dp"
   android:layout_height="180dp"
   android:layout_marginBottom="16dp"
   android:layout_marginLeft="16dp"
   android:layout_marginRight="16dp"
   android:layout_marginTop="70dp"
   app:cardBackgroundColor="?attr/colorPrimaryDark"
   app:cardCornerRadius="4dp"
   app:cardElevation="4dp">

   <LinearLayout
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:layout_gravity="bottom"
       android:background="#FFFFFF"
       android:orientation="vertical"
       android:padding="8dp">

       <TextView
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:padding="2dp"
           android:text="@string/shr_product_title"
           android:textAppearance="?attr/textAppearanceHeadline6" />

       <TextView
           android:layout_width="match_parent"
           android:layout_height="wrap_content"
           android:padding="2dp"
           android:text="@string/shr_product_description"
           android:textAppearance="?attr/textAppearanceBody2" />
   </LinearLayout>
</com.google.android.material.card.MaterialCardView>
```

运行效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582703456801-37d2e31b-08dc-49c2-8f2b-cb9e45f399d0.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=64300&status=done&style=none&width=405)

#### 网格Card
如果有多个Card需要显示，则他们会被分为一个或多个集合，网格中的Card处于同一个面，意味着他们彼此的高度统一。

查看官方提供的shr_product_card.xml
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:cardBackgroundColor="@android:color/white"
    app:cardElevation="2dp"
    app:cardPreventCornerOverlap="true">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <com.android.volley.toolbox.NetworkImageView
            android:id="@+id/product_image"
            android:layout_width="match_parent"
            android:layout_height="@dimen/shr_product_card_image_height"
            android:background="?attr/colorPrimaryDark"
            android:scaleType="centerCrop" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="16dp">

            <TextView
                android:id="@+id/product_title"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="@string/shr_product_title"
                android:textAppearance="?attr/textAppearanceHeadline6" />

            <TextView
                android:id="@+id/product_price"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:text="@string/shr_product_description"
                android:textAppearance="?attr/textAppearanceBody2" />
        </LinearLayout>
    </LinearLayout>
</com.google.android.material.card.MaterialCardView>
```

在这个官方提供的布局中，包含了一张图片及两段文字。

为了应用这个网格，将shr_product_grid_fragment内的Card替换为如下代码：
```kotlin
<androidx.core.widget.NestedScrollView
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:layout_marginTop="56dp"
   android:background="@color/productGridBackgroundColor"
   android:paddingStart="@dimen/shr_product_grid_spacing"
   android:paddingEnd="@dimen/shr_product_grid_spacing"
   app:layout_behavior="@string/appbar_scrolling_view_behavior">

   <androidx.recyclerview.widget.RecyclerView
       android:id="@+id/recycler_view"
       android:layout_width="match_parent"
       android:layout_height="match_parent" />

</androidx.core.widget.NestedScrollView>
```

修改ProductGridFragment.kt的onCreate
```kotlin
override fun onCreateView(
           inflater: LayoutInflater, container: ViewGroup?, savedInstanceState: Bundle?): View? {
       // Inflate the layout for this fragment with the ProductGrid theme
       val view = inflater.inflate(R.layout.shr_product_grid_fragment, container, false)

       // Set up the toolbar.
       (activity as AppCompatActivity).setSupportActionBar(view.app_bar)

       // Set up the RecyclerView
       view.recycler_view.setHasFixedSize(true)
       view.recycler_view.layoutManager = GridLayoutManager(context, 2, RecyclerView.VERTICAL, false)
       val adapter = ProductCardRecyclerViewAdapter(
               ProductEntry.initProductEntryList(resources))
       view.recycler_view.adapter = adapter
       val largePadding = resources.getDimensionPixelSize(R.dimen.shr_product_grid_spacing)
       val smallPadding = resources.getDimensionPixelSize(R.dimen.shr_product_grid_spacing_small)
       view.recycler_view.addItemDecoration(ProductGridItemDecoration(largePadding, smallPadding))

       return view;
   }
```

效果如图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582707285500-48a72dd8-7328-471b-a05a-0e6e283cb0cb.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=93155&status=done&style=none&width=405)

#### 修改Card内容
ViewHolder包含了每个卡片的视图，在ViewHolder内添加控件：
```kotlin
class ProductCardViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView) {

   var productImage: NetworkImageView = itemView.findViewById(R.id.product_image)
   var productTitle: TextView = itemView.findViewById(R.id.product_title)
   var productPrice: TextView = itemView.findViewById(R.id.product_price)
}
```

在ProductCardRecyclerViewAdapter中的onBindViewHolder指定了绑定在控件上的内容，可以如此修改：

```kotlin
override fun onBindViewHolder(holder: ProductCardViewHolder, position: Int) {
   if (position < productList.size) {
       val product = productList[position]
       holder.productTitle.text = product.title
       holder.productPrice.text = product.price
       ImageRequester.setImageFromUrl(holder.productImage, product.url)
   }
}
```

以上代码可以使RecyclerView适配器对每张Card进行处理。

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582707794723-53484933-64d8-4228-a758-0a203864cbde.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=594897&status=done&style=none&width=405)
