# Material 高级组件

[https://github.com/material-components/material-components-android-codelabs/archive/104-starter.zip](https://github.com/material-components/material-components-android-codelabs/archive/104-starter.zip)

# Android界面
### 添加菜单
这里运用到的菜单为背景菜单，由背景层和正面层组成，背景层用于显示动作和过滤器，正面层用于显示内容。

#### 隐藏网格内容
在中`shr_product_grid_fragment.xml`，将`android:visibility="gone"`属性添加到`NestedScrollView`用于隐藏视图，以便更直观的看见菜单内容。

shr_product_grid_fragment.xml
```kotlin
<androidx.core.widget.NestedScrollView
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:layout_marginTop="56dp"
   android:background="@color/productGridBackgroundColor"
   android:elevation="8dp"
   android:visibility="gone"
   app:layout_behavior="@string/appbar_scrolling_view_behavior">
```


#### 添加样式
在`style.xml`内添加如下样式：
```kotlin
<style name="Widget.Shrine.Backdrop" parent="">
   <item name="android:background">?attr/colorAccent</item>
</style>
```

#### 添加用于显示菜单的布局
在`shr_product_grid_fragment.xml`中，将以下内容添加到`AppBarLayout`前面：
```kotlin
<LinearLayout
   style="@style/Widget.Shrine.Backdrop"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:gravity="center_horizontal"
   android:orientation="vertical"
   android:paddingTop="100dp"
   android:paddingBottom="100dp">

</LinearLayout>
```

#### 新建菜单
在res->layout内新建菜单布局，命名为shr_backdrop.xml，代码如下：
```kotlin
<?xml version="1.0" encoding="utf-8"?>
<merge xmlns:android="http://schemas.android.com/apk/res/android">

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_featured_label" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_apartment_label" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_accessories_label" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_shoes_label" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_tops_label" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_bottoms_label" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_dresses_label" />

   <View
       android:layout_width="56dp"
       android:layout_height="1dp"
       android:layout_margin="16dp"
       android:background="?android:attr/textColorPrimary" />

   <com.google.android.material.button.MaterialButton
       style="@style/Widget.Shrine.Button.TextButton"
       android:layout_width="wrap_content"
       android:layout_height="wrap_content"
       android:text="@string/shr_account_label" />

</merge>
```

将以上内容引入到先前添加的LinearLayout中：
```kotlin
<include layout="@layout/shr_backdrop" />
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582874525557-36d10c34-ebad-4309-b4d4-7427a7f2e39d.png#align=left&display=inline&height=608&name=image.png&originHeight=1484&originWidth=810&size=85838&status=done&style=none&width=332)

### 美化菜单
首先将先前添加的`android:visibility="gone"`属性删除。
根据官方解释，Material Design的可以允许用户自定义形状。

#### 添加shape
根据前面的官方说明，我们可以在`NestedScrollView`上添加形状，但是，该特性只能用于Android Marshmallow或更高版本。

为`NestedScrollView`添加ID，并在`ProductGridFragment.kt`的onCreateView中修改形状。
shr_product_grid_fragment.xml
```kotlin
<androidx.core.widget.NestedScrollView
   android:id="@+id/product_grid"
   android:layout_width="match_parent"
   android:layout_height="match_parent"
   android:layout_marginTop="56dp"
   android:background="@color/productGridBackgroundColor"
   android:elevation="8dp"
   app:layout_behavior="@string/appbar_scrolling_view_behavior">
```

`ProductGridFragment.kt`
```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
       view.product_grid.background = context?.getDrawable(R.drawable.shr_product_grid_background_shape)
}
```

最后更新颜色配置：
```kotlin
<color name="productGridBackgroundColor">#FFFBFA</color>
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582875809531-6e9f240e-1b1f-4362-853b-4bd69f47e83a.png#align=left&display=inline&height=568&name=image.png&originHeight=1484&originWidth=810&size=474754&status=done&style=none&width=310)

这个分层的结构很有意思，可以提升界面布局中的自由度，目前的效果还不具备动作。

### 添加动作
#### 向菜单按钮添加显示动作
像`ProductGridFragment.kt`的onCreateView中添加事件监听：
```kotlin
view.app_bar.setNavigationOnClickListener(NavigationIconClickListener(activity!!, view.product_grid))
```

NavigationIconClickListener.kt：
```kotlin
class NavigationIconClickListener @JvmOverloads internal constructor(
        private val context: Context, private val sheet: View, private val interpolator: Interpolator? = null,
        private val openIcon: Drawable? = null, private val closeIcon: Drawable? = null) : View.OnClickListener {

    private val animatorSet = AnimatorSet()
    private val height: Int
    private var backdropShown = false

    init {
        val displayMetrics = DisplayMetrics()
        (context as Activity).windowManager.defaultDisplay.getMetrics(displayMetrics)
        height = displayMetrics.heightPixels
    }

    override fun onClick(view: View) {
        backdropShown = !backdropShown

        // Cancel the existing animations
        animatorSet.removeAllListeners()
        animatorSet.end()
        animatorSet.cancel()
        
        updateIcon(view)
        
        val translateY = height - context.resources.getDimensionPixelSize(R.dimen.shr_product_grid_reveal_height)

        val animator = ObjectAnimator.ofFloat(sheet, "translationY", (if (backdropShown) translateY else 0).toFloat())
        animator.duration = 500
        if (interpolator != null) {
            animator.interpolator = interpolator
        }
        animatorSet.play(animator)
        animator.start()
    }
    
    private fun updateIcon(view: View) {
        if (openIcon != null && closeIcon != null) {
            if (view !is ImageView) {
                throw IllegalArgumentException("updateIcon() must be called on an ImageView")
            }
            if (backdropShown) {
                view.setImageDrawable(closeIcon)
            } else {
                view.setImageDrawable(openIcon)
            }
        }
    }
}
```

以上代码的作用主要包括，点击后，当前层下降200dp的动画效果和改变icon的显示，这里因为没有填入变更的icon，所以icon不会进行变化。

根据官方教程，在调用NavigationIconClickListener的时候，可以填入第三个参数AccelerateDecelerateInterpolator()改变时间曲线，不过我没有看出啥不同。。。

关于interpolator插值器，可以参考[https://www.jianshu.com/p/1f2501840db8](https://www.jianshu.com/p/1f2501840db8)

#### 改变菜单icon
经过对NavigationIconClickListener的解读，不难看出，要达到改变图标的效果，其实只需要将点击前后的两个图标分别填入第四第五个参数即可达到效果，因此需要对setNavigationOnClickListener进行修改，代码如下：
```kotlin
view.app_bar.setNavigationOnClickListener(NavigationIconClickListener(
       activity!!,
       view.product_grid,
       AccelerateDecelerateInterpolator(),
       ContextCompat.getDrawable(context!!, R.drawable.shr_branded_menu), // Menu open icon
       ContextCompat.getDrawable(context!!, R.drawable.shr_close_menu))) // Menu close icon
```

还要对修改布局文件的初始icon引用：
```kotlin
<androidx.appcompat.widget.Toolbar
   android:id="@+id/app_bar"
   style="@style/Widget.Shrine.Toolbar"
   android:layout_width="match_parent"
   android:layout_height="?attr/actionBarSize"
   android:paddingStart="12dp"
   android:paddingLeft="12dp"
   android:paddingEnd="12dp"
   android:paddingRight="12dp"
   app:contentInsetStart="0dp"
   app:navigationIcon="@drawable/shr_branded_menu"
   app:title="@string/shr_app_name" />
```

最终效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582877351596-a0a66bf3-d265-466c-bd6a-72c76aabd0cc.png#align=left&display=inline&height=660&name=image.png&originHeight=1484&originWidth=810&size=475992&status=done&style=none&width=360)![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1582877386805-1d4738a6-ce90-4b24-8602-c4a7d403715c.png#align=left&display=inline&height=660&name=image.png&originHeight=1484&originWidth=810&size=127784&status=done&style=none&width=360)
