# XLayout布局

XLayout是一种相对于原生的OSX编程布局更方便快速的方案。
GItHub地址：[https://github.com/HsiangHo/XLayout#](https://github.com/HsiangHo/XLayout#)

# 环境需求：

- macOS / OS X 10.10 +
- Xcode 10.0+
- Swift 5.0+

# 图解：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1584696973393-1056e54d-c400-4152-ac91-1cc35a754b87.png#align=left&display=inline&height=495&name=image.png&originHeight=990&originWidth=1764&size=72685&status=done&style=none&width=882)

运用XLayout后的布局更像是安卓的相对布局，可以允许用户为需要定位的View设置四个方向的相对位置，在脱离stoaryboard而使用代码进行布局的时候可以方便很多。
如上图所示，View2的位置在View1的右侧，此时View1就是View2的参照物。

## 如何使用：

### 相对设定：
如上图所示的例子，代码布局如下：
```swift
view1.xLayout.leading(20).bottom(-15).width(150).height(100)
view2.xLayout.leading(view1.trailing + 80).height(30).trailing(-15).top(40)
```
※：以上的布局代码需要执行在addSubview之后执行，否则无效。
#### 属性说明：

- leading： 表示左边距
- trailing： 表示右边距
- top：表示上边距
- bottom： 表示底部边距
- width： 表示宽度
- height： 表示高度

### 赋值设定：
若不使用函数的方式设定，也可以直接对其属性进行设置：
```swift
view1.width == 150
view1.height == 100
view1.leading == 20
view1.bottom == -15
view2.leading == view1.trailing + 80
view2.height == 30
view2.trailing == -15
view2.top == 40
```

需要注意的是：在对其各项属性进行赋值时，需要使用“==”而不是常规的“=”。

### 整体设定：
除了以上的设定方式，XLayout也可以通过窗体的整体位置进行设置，代码如下：
```swift
// the | means superView and the - means space
view1.width == 150
view1.height == 100
view1.visualLayout(.H(|-20-view1), .V(view1-15-|))

view2.height == 30
view2.visualLayout(.H(view1-80-view2-15-|), .V(|-40-view2))
```

可以看见visualLayout非常有意思，visualLayout使用“|”来表示superView的边界，“-”来表示空格，H表示水平，V表示垂直。




