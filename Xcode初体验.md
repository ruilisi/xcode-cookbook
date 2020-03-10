# Xcode初体验

# 项目的创建
完成相关部署后，打开Xcode，选择第二个选项进行IOS工程的创建。
界面如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583045534444-efd07137-88ad-4815-b275-def7a7612d89.png#align=left&display=inline&height=526&name=image.png&originHeight=1052&originWidth=1460&size=209393&status=done&style=none&width=730)
默认选项为单页面模板，点击Next。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583045816592-5baed87f-f60e-4191-aeb6-e407f9ab25ae.png#align=left&display=inline&height=526&name=image.png&originHeight=1052&originWidth=1460&size=192280&status=done&style=none&width=730)

从上到下分别为：

- 项目名称
- 团队
- 组织名称
- 组织ID

Bundle ID为组织ID和项目名称组合生成，要求具有唯一性。

语言选择Swift。

创建成功后的界面如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583046112107-c26f0610-ebdc-4006-8b8e-9109ed2a6ef4.png#align=left&display=inline&height=899&name=image.png&originHeight=1798&originWidth=2878&size=1171130&status=done&style=none&width=1439)


# 文件结构
创建项目后的结构如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583046639255-50180df9-4024-415a-aaeb-b55bdff59ac7.png#align=left&display=inline&height=280&name=image.png&originHeight=560&originWidth=596&size=260966&status=done&style=none&width=298)
其中，swift结尾的为代码文件，Assets.xcassets为资源文件，LaunchScreen.storyboard为设计界面。

# Swift UI
Swift UI似乎是现在苹果主推的一种布局方式。

在新建的工程里，有个名为ContentView.swift的文件，其代码如下：
```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        Text("Hello World!")
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

函数ContentView内部的body参数内包含了一个Text组件，猜测这里的代码就是Swift UI的核心。

## 容器
### HStack
HStack为容器组件，他的排列方向是水平的，类似于Android中水平排列的LinearLayout。
试着操作一下：
```swift
struct ContentView: View {
    var body: some View {
        HStack{
            Text("Hello")
            Text(" ")
            Text("world!")
        }
    }
}
```
将三个Text用HStack包裹起来，可在右侧预览到的情况如图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583047846441-15818d56-f2e7-4b70-bb6e-94ca0ba736fa.png#align=left&display=inline&height=460&name=image.png&originHeight=920&originWidth=508&size=24315&status=done&style=none&width=254)

### VStack
和HStack相同，VStack也是一个容器组件，惟一的区别在于VStack是垂直的，而HStack是水平的。
代码如下：
```swift
struct ContentView: View {
    var body: some View {
        VStack{
            Text("Hello")
            Text(" ")
            Text("world!")
        }
    }
}
```
效果如图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583047985096-082377d7-2737-4248-96ab-0dc9c3eab8a2.png#align=left&display=inline&height=453&name=image.png&originHeight=906&originWidth=518&size=25693&status=done&style=none&width=259)

组合使用排列一个矩阵：
```swift
struct ContentView: View {
    var body: some View {
        VStack{
            HStack{
                Text("1")
                Text("2")
                Text("3")
            }
            HStack{
                Text("4")
                Text("5")
                Text("6")
            }
            HStack{
                Text("7")
                Text("8")
                Text("9")
            }
        }
    }
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583048104769-1b516e8f-a167-48f8-93fb-2bd0570265ad.png#align=left&display=inline&height=448&name=image.png&originHeight=896&originWidth=458&size=18551&status=done&style=none&width=229)


## 试着做一个计算器界面：
### Text基本属性
首先需要了解Text的基本属性
代码如下：
```swift
var body: some View {
    VStack{
        Text("0")
            .font(.system(size:60))
            .foregroundColor(Color.white)
    }
}
```
font指定了Text的字体和大小，这里制定了字体大小为60

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583048892459-777bd064-018b-4155-b7d3-d21dca2335b7.png#align=left&display=inline&height=454&name=image.png&originHeight=908&originWidth=468&size=17690&status=done&style=none&width=234)
出现两个问题

- 背景色和字体颜色都为白色，无法体现字体内容。
- 字体出现在了屏幕最中间。

### VStack基本属性
为了解决以上两个问题，需要设置VStack的尺寸及背景色，代码如下：
```swift
var body: some View {
    VStack{
        Text("0")
            .font(.system(size:60))
            .foregroundColor(Color.white)
    }
    .frame(maxWidth: .infinity,maxHeight: .infinity)
    .background(Color.black)
    .edgesIgnoringSafeArea(.all)
}
```
这里设置了三个属性：

- 第一个属性设置了VStack的最大高度和最大宽度
- 第二个属性设置了VStack区域的背景色
- 第三个属性设置了忽略安全区域

值得注意的是，设置背景色需要写在设置安全区域的前面，否则颜色无法填充到安全区域。

### 添加资源
可以在资源文件内添加一种颜色，步骤为：点击Assets.xcassets->New Color Set
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583049441693-c4886c5b-a7a0-4065-944b-d297dd91f8b6.png#align=left&display=inline&height=374&name=image.png&originHeight=748&originWidth=2320&size=124894&status=done&style=none&width=1160)

与安卓的不同之处在于，这里可以设置Appearances来设置他的模式

创建好的Color如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583049617019-60bbf024-5625-44e8-ad73-3ec3acc5cf33.png#align=left&display=inline&height=612&name=image.png&originHeight=1224&originWidth=2354&size=275212&status=done&style=none&width=1177)

调用方式如下：
```swift
.background(Color("calc_bg"))
```

值得一提的是，在ContentView.swift内，可以通过点击相关组件后，直接修改Xcode右侧的内容来达到设置其值得效果。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583049678282-1a2e20e8-018c-46b2-990a-79fb49b7acde.png#align=left&display=inline&height=646&name=image.png&originHeight=1292&originWidth=538&size=72108&status=done&style=none&width=269)

### 自定义组件
为了实现计算器上的原型按钮，需要自定义一个组件，代码如下：
```swift
struct CustomButton: View {
    let text : String
    var body: some View{
        Text(text)
            .frame(width: 80, height: 80)
            .font(.title)
    }
}
```

使用自定义组件显示第一排按钮：
```swift
HStack{
    CustomButton(text:"AC")
    .background(Color("s_operator_bg"))
    .foregroundColor(/*@START_MENU_TOKEN@*/Color("s_operator_fg")/*@END_MENU_TOKEN@*/)
	.cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
                
    CustomButton(text:"+/-")
	.background(Color("s_operator_bg"))
	.foregroundColor(/*@START_MENU_TOKEN@*/Color("s_operator_fg")/*@END_MENU_TOKEN@*/)
	.cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
                
	CustomButton(text:"%")
	.background(Color("s_operator_bg"))
	.foregroundColor(/*@START_MENU_TOKEN@*/Color("s_operator_fg")/*@END_MENU_TOKEN@*/)
	.cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
                
	CustomButton(text:"÷")
	.background(Color("operator_bg"))
	.foregroundColor(/*@START_MENU_TOKEN@*/Color("operator_fg")/*@END_MENU_TOKEN@*/)
	.cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583053875097-ae3580d5-a34e-4ee0-a84e-9c9732b3a4f5.png#align=left&display=inline&height=454&name=image.png&originHeight=908&originWidth=452&size=25100&status=done&style=none&width=226)

效果是出来了，但是有个问题，代码过于繁琐，可以使用ViewModifier简化代码。
```swift
struct SOperatorModifier: ViewModifier {
    func body(content: Content) -> some View {
        content
            .frame(width: 80, height: 80)
            .font(.title)
            .background(Color("s_operator_bg"))
            .foregroundColor(/*@START_MENU_TOKEN@*/Color("s_operator_fg")/*@END_MENU_TOKEN@*/)
            .cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
    }
}

struct OperatorModifier: ViewModifier {
    func body(content: Content) -> some View {
        content
            .frame(width: 80, height: 80)
            .font(.title)
            .background(Color("operator_bg"))
            .foregroundColor(/*@START_MENU_TOKEN@*/Color("operator_fg")/*@END_MENU_TOKEN@*/)
            .cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
    }
}
```

调用：
```swift
 HStack{
     CustomButton(text:"AC")
     	.modifier(SOperatorModifier())
           
     CustomButton(text:"+/-")
     	.modifier(SOperatorModifier())
	
     CustomButton(text:"%")
     	.modifier(SOperatorModifier())
             
     CustomButton(text:"÷")
     	.modifier(OperatorModifier())
 }
```

这样看起来舒服多了，同样依次设置每一行的按钮。

### 构造函数
计算其中的按钮0是一个比较大的按钮，可以通过构造函数来解决这个问题：
```swift
struct NumberModifier: ViewModifier {
    let biggerWidth : Bool
    
    init(bigger:Bool = false) {
        biggerWidth = bigger
    }
    
    func body(content: Content) -> some View {
        content
            .frame(width: biggerWidth ? 180 : 80, height: 80)
            .font(.title)
            .background(Color("number_bg"))
            .foregroundColor(/*@START_MENU_TOKEN@*/Color("number_fg")/*@END_MENU_TOKEN@*/)
            .cornerRadius(/*@START_MENU_TOKEN@*/40.0/*@END_MENU_TOKEN@*/)
    }
}
```

调用：
```swift
CustomButton(text:"0")
                    .modifier(NumberModifier(bigger: true))
```

**设置间距**
为每行之间设置一个固定的间距，只需要在VStack内加入spacing属性即可：
```swift
VStack(spacing: 20)
```

**修改数字显示区域：**
```swift
Text("0")
	.frame(maxWidth:.infinity, maxHeight: 130, alignment: .trailing)
	.padding(.trailing, 30)
	.font(.system(size:60))
	.foregroundColor(Color("result_fg"))
```

最终效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583057064850-78c6e49b-6f3a-4d75-8344-f74593c2c92f.png#align=left&display=inline&height=679&name=image.png&originHeight=1358&originWidth=698&size=110250&status=done&style=none&width=349)


