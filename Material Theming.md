# Material Theming

[https://github.com/material-components/material-components-android-codelabs/archive/103-starter.zip](https://github.com/material-components/material-components-android-codelabs/archive/103-starter.zip)

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
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582784397622-dd323036-ad53-49fe-89ae-565997ac5665.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=592943&status=done&style=none&width=405)


### 修改商品界面主题
#### 修改配色
color.xml：
```kotlin
<color name="colorPrimaryDark">#FBB8AC</color>
<color name="colorAccent">#FEDBD0</color>
```

在启动程序，界面如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582784656095-d217f9c4-ea5e-4aa8-967f-53019de417b0.png#align=left&display=inline&height=742&name=image.png&originHeight=1484&originWidth=810&size=595118&status=done&style=none&width=405)
白色的字体与背景色的米粉色色系相近，导致了图标显示不清晰，则需要通过修改styles.xml文件对图标色系进行调整。

#### 配置style.xml
将以下代码加入到styles内：
```kotlin
<item name="android:windowLightStatusBar" tools:targetApi="m">true</item>
```

直接引入会导致报错，原因为未引入必要的工具，引用代码如下：
```kotlin
<resources xmlns:tools="http://schemas.android.com/tools">
```

#### 配置toolbar的图标颜色
color.xml
```kotlin
<color name="textColorPrimary">#442C2E</color>
<color name="toolbarIconColor">@color/textColorPrimary</color>
```

为style.xml配置默认文本颜色：
```kotlin
<item name="android:textColorPrimary">@color/textColorPrimary</item>
```

将样式中的`android:theme`属性`Widget.Shrine.Toolbar`更改为`Theme.Shrine`。
```kotlin
<item name="android:theme">@style/Theme.Shrine</item>
```

运行效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582786266261-03ba017b-ad08-41de-bbdf-8f51611bbb72.png#align=left&display=inline&height=627&name=image.png&originHeight=1484&originWidth=810&size=70763&status=done&style=none&width=342)![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582786183430-3e4b4072-e642-41c2-83e0-530da153eb13.png#align=left&display=inline&height=627&name=image.png&originHeight=1484&originWidth=810&size=600804&status=done&style=none&width=342)

显然，登录界面和商品界面的色系并不搭配，为了达到良好视觉效果，下一步需要对登录界面进行调整。

### 修改登录界面主题
#### 添加文本色调
将以下代码加入color.xml
```kotlin
<color name="textInputOutlineColor">#FBB8AC</color>
```

#### 修改输入框样式
在style.xml中添加两种新的样式：
```kotlin
<style name="Widget.Shrine.TextInputLayout" parent="Widget.MaterialComponents.TextInputLayout.OutlinedBox">
   <item name="hintTextAppearance">@style/TextAppearance.Shrine.TextInputLayout.HintText</item>
   <item name="hintTextColor">@color/textColorPrimary</item>
   <item name="android:paddingBottom">8dp</item>
   <item name="boxStrokeColor">@color/textInputOutlineColor</item>
</style>

<style name="TextAppearance.Shrine.TextInputLayout.HintText" parent="TextAppearance.MaterialComponents.Subtitle2">
   <item name="android:textColor">?android:attr/textColorPrimary</item>
</style>
```

为登录页面布局中的两个输入框附加以下样式：
```kotlin
style="@style/Widget.Shrine.TextInputLayout"
```

#### 设置按钮样式
```kotlin
<style name="Widget.Shrine.Button" parent="Widget.MaterialComponents.Button">
   <item name="android:textColor">?android:attr/textColorPrimary</item>
   <item name="backgroundTint">?attr/colorPrimaryDark</item>
</style>

<style name="Widget.Shrine.Button.TextButton" parent="Widget.MaterialComponents.Button.TextButton">
   <item name="android:textColor">?android:attr/textColorPrimary</item>
</style>
```

分别为cancel按钮和next按钮添加如下样式：
```kotlin
<com.google.android.material.button.MaterialButton
       android:id="@+id/next_button"
       style="@style/Widget.Shrine.Button"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_alignParentEnd="true"
       android:layout_alignParentRight="true"
       android:text="@string/shr_button_next" />
<com.google.android.material.button.MaterialButton
       android:id="@+id/cancel_button"
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:layout_marginEnd="12dp"
       android:layout_marginRight="12dp"
       android:layout_toStartOf="@id/next_button"
       android:layout_toLeftOf="@id/next_button"
       android:text="@string/shr_button_cancel" />
```

效果展示：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582786899992-1aca4d84-97c0-42b1-9222-9b508c5b300d.png#align=left&display=inline&height=632&name=image.png&originHeight=1484&originWidth=810&size=73053&status=done&style=none&width=345)

### 修改字体和样式
除了上面的对色彩和样式的调整，也可以对字体进行自定义调整，具体方式如下
#### 设置顶部toolBar的样式
将以下代码加入styles.xml
```kotlin
<style name="Widget.Shrine.Toolbar" parent="Widget.AppCompat.Toolbar">
   <item name="android:background">?attr/colorAccent</item>
   <item name="android:theme">@style/Theme.Shrine</item>
   <item name="popupTheme">@style/ThemeOverlay.AppCompat.Light</item>
   <item name="titleTextAppearance">@style/TextAppearance.Shrine.Toolbar</item>
</style>

<style name="TextAppearance.Shrine.Toolbar" parent="TextAppearance.MaterialComponents.Button">
   <item name="android:textSize">16sp</item>
</style>
```

#### 修改Card字体
修改shr_product_card.xml的标题字体，
并为两个TextView加入android:textAlignment="center"属性
```kotlin
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
       android:textAlignment="center"
       android:textAppearance="?attr/textAppearanceSubtitle2" />

   <TextView
       android:id="@+id/product_price"
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:text="@string/shr_product_description"
       android:textAlignment="center"
       android:textAppearance="?attr/textAppearanceBody2" />
</LinearLayout>
```

效果如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582788829344-70aaec0e-99e1-4b62-8ada-1e1cfa949f8d.png#align=left&display=inline&height=638&name=image.png&originHeight=1484&originWidth=810&size=602847&status=done&style=none&width=348)

#### 修改登录界面字体
添加以下样式：
```kotlin
<style name="TextAppearance.Shrine.Title" parent="TextAppearance.MaterialComponents.Headline4">
   <item name="textAllCaps">true</item>
   <item name="android:textStyle">bold</item>
   <item name="android:textColor">?android:attr/textColorPrimary</item>
</style>
```

为shr_login_fragment中的标题添加如下代码：
```kotlin
android:textAppearance="@style/TextAppearance.Shrine.Title" 
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582789318685-35cded92-5e38-4d29-8410-673f51dc7295.png#align=left&display=inline&height=612&name=image.png&originHeight=1484&originWidth=810&size=75267&status=done&style=none&width=334)


### 更改布局
我们可以修改Card的布局方式，满足自己的不同需求。

#### 使用交错的RecyclerView适配器
修改ProductGridFragment.kt的onCreateView方法：
```kotlin
view.recycler_view.setHasFixedSize(true)
val gridLayoutManager = GridLayoutManager(context, 2, RecyclerView.HORIZONTAL, false)
gridLayoutManager.spanSizeLookup = object : GridLayoutManager.SpanSizeLookup() {
   override fun getSpanSize(position: Int): Int {
       return if (position % 3 == 2) 2 else 1
   }
}
view.recycler_view.layoutManager = gridLayoutManager
val adapter = StaggeredProductCardRecyclerViewAdapter(
       ProductEntry.initProductEntryList(resources))
view.recycler_view.adapter = adapter
val largePadding = resources.getDimensionPixelSize(R.dimen.shr_staggered_product_grid_spacing_large)
val smallPadding = resources.getDimensionPixelSize(R.dimen.shr_staggered_product_grid_spacing_small)
view.recycler_view.addItemDecoration(ProductGridItemDecoration(largePadding, smallPadding))
```

修改shr_product_grid_fragment.xml，将padding属性删除：
```kotlin
<androidx.core.widget.NestedScrollView
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:layout_marginTop="56dp"
   android:background="@color/productGridBackgroundColor"
   app:layout_behavior="@string/appbar_scrolling_view_behavior"
   android:elevation="6dp">
```

修改ProductGridItemDecoration.kt的getItemOffsets()方法，以达到修改边距的效果：
```kotlin
override fun getItemOffsets(outRect: Rect, view: View,
                           parent: RecyclerView, state: RecyclerView.State?) {
   outRect.left = smallPadding
   outRect.right = largePadding
}
```

最终效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582790444831-278df443-d619-4899-8280-2022b855b60a.png#align=left&display=inline&height=627&name=image.png&originHeight=1484&originWidth=810&size=472174&status=done&style=none&width=342)

滚动从垂直滚动变为横向滚动。
