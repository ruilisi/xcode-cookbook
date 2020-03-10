# Storyboard页面跳转

# 新建Controller
单个页面是无法进行跳转的，因此需要新建一个ViewController来进行跳转。

点击右上角的加号跳出如下窗口，选择ViewController拖放进入Main.board。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293179552-d4b5ce46-9946-4c9c-86e0-7b04f8995f59.png#align=left&display=inline&height=496&name=image.png&originHeight=992&originWidth=1362&size=911013&status=done&style=none&width=681)

向Entry Point的Controller中加入一个Button作为跳转的按钮。


得到如下结果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293320563-8d1fa199-af06-4a4e-8a74-894a441594a5.png#align=left&display=inline&height=497&name=image.png&originHeight=994&originWidth=1034&size=46259&status=done&style=none&width=517)

# 添加Segue
和安卓不同，Storyboard的页面跳转操作相对简易，只需在Button上拖动一个Segue到新建的ViewController即可。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293405118-6a564fe6-c155-44de-b837-3fe6b8f1a4d6.png#align=left&display=inline&height=170&name=image.png&originHeight=340&originWidth=400&size=60662&status=done&style=none&width=200)

添加Segue需要选择一个Action。

常用的Action为Show和Present Modally。Show用在具有导航栏的页面时，跳到下一页到时候会自带导航栏，并且带一个返回按钮。Present Modally则没有这个功能。
ActionSegue仅支持IOS7以上的设备，如果需要支持更低版本，应使用Non-Adaptive Action Segue。

官方说明图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293560626-442a6c5e-0d83-415e-87fe-4e3ee1f95cf2.png#align=left&display=inline&height=631&name=image.png&originHeight=1262&originWidth=1376&size=297197&status=done&style=none&width=688)

这里选择show。

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293639833-1ceab63a-f2cb-4c45-8267-51c2bbae86b1.png#align=left&display=inline&height=761&name=image.png&originHeight=1522&originWidth=768&size=57128&status=done&style=none&width=384)

跳转成功，但是这种跳转方式并不是我想要的。

## 属性
Segue的属性如下：

### Identifier：用于定义Segue的标识
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583294384320-7a2e0471-20ce-42ec-9e22-076be414639d.png#align=left&display=inline&height=247&name=image.png&originHeight=494&originWidth=510&size=49630&status=done&style=none&width=255)

### Kind：跳转的方式
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293914921-b2c850e8-eb2c-4512-942f-de5e634adaf3.png#align=left&display=inline&height=208&name=image.png&originHeight=416&originWidth=358&size=190668&status=done&style=none&width=179)
改为使用Present Modally。

### Presentation：跳转后的样式
修改跳转后的显示方式：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583293962867-8ae1f843-92ad-486f-b098-f52ff8c4545c.png#align=left&display=inline&height=160&name=image.png&originHeight=320&originWidth=350&size=158238&status=done&style=none&width=175)

### Transition：跳转界面的方式
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583294337879-4eb811b8-78af-4515-aee4-6c5599e30af4.png#align=left&display=inline&height=100&name=image.png&originHeight=200&originWidth=350&size=90033&status=done&style=none&width=175)

| Cover Vertical | 水平上移切换 |
| --- | --- |
| Filp Horizontal | 从右到左翻转 |
| Cross Dissolve | 闪现 |
| Partial Curl | 从下往上翻页 |


# Push跳转
Push跳转必须在一个Navigation Controller内实现。

删除原有的View Controller，新建Navigation Controller，并设置Entry Point：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583294910175-ca8a1fed-ab04-4eae-be48-ec13649772e3.png#align=left&display=inline&height=555&name=image.png&originHeight=1110&originWidth=1216&size=97600&status=done&style=none&width=608)

把原有的RootView删除，新建一个View Controller，添加Segue，并设置为root view controller。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583294970748-eb88eb1e-e9f5-4a16-ae7c-b79bb6c06cd1.png#align=left&display=inline&height=176&name=image.png&originHeight=352&originWidth=348&size=31316&status=done&style=none&width=174)

之后在新建一个View Controller，添加Segue，类型设置为push。

完成后的效果如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583295114198-7cb2329c-2830-4808-b7cc-4924101155b7.png#align=left&display=inline&height=426&name=image.png&originHeight=852&originWidth=958&size=54954&status=done&style=none&width=479)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583295138562-7645dbe3-b199-49de-9e1d-b433f27de48d.png#align=left&display=inline&height=531&name=image.png&originHeight=1522&originWidth=768&size=68545&status=done&style=none&width=268) ![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583295148125-7f23cc6d-ae02-4c56-8747-24bb85a9ba18.png#align=left&display=inline&height=535&name=image.png&originHeight=1522&originWidth=768&size=68254&status=done&style=none&width=270)

---

# 手动跳转
先前介绍的跳转方式都为自动跳转，接下来试试手动跳转。

参考：[https://www.jianshu.com/p/2fb5f4b97b4d](https://www.jianshu.com/p/2fb5f4b97b4d)
参考：[https://blog.csdn.net/qq_35776048/article/details/54562652](https://blog.csdn.net/qq_35776048/article/details/54562652)

按住control键，拖动首页的View Controller到需要跳转的界面。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583300472298-35a0d569-78d7-4141-a3cb-7af8f342381f.png#align=left&display=inline&height=76&name=image.png&originHeight=152&originWidth=530&size=14426&status=done&style=none&width=265)
为Segue设置Identifier。

跟前面一样，在ViewController中为Button添加事件，加入跳转代码：
```swift
@IBAction func btn_click(_ sender: UIButton) {
	self.performSegue(withIdentifier: "second2view", sender: self)
}
```

_**segue显示模式**_
_在iPhone上可以用到的有modal、push和custom，其他还有几种是iPad上用的：_
_1、modal：模态地加载视图控制器，最常用的方式，类似present和dismiss；_
_2、push：使用导航栏压进新的视图控制器，类似push和pop，要使用这个模式，跳转的源视图，也就是主控制器必须是Navigation Controller，否则会报错；_
_3、custom：用户自定义。_
_
### 参数传递：
```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
	if segue.identifier == "second2view" {
		let controller = segue.destination as!SecondViewController
			controller.str = "传递的参数"
	}
}
```
注意：第四行处的str为对应Controller的变量。

通过覆写prepare函数即可实现参数传递，注意，和安卓不同，这里的跳转不通过某个固定的控件，都通过prepare实现。

修改参数：
```swift
override func viewDidLoad() {
	label_name.text = str
}
```

在viewDidLoad函数内可以对控件的值初始化。

![](https://cdn.nlark.com/yuque/__puml/09aad6cfe3847310ae42fa369f413218.svg#lake_card_v2=eyJjb2RlIjoiQHN0YXJ0dW1sXG5cbnN0YXJ0XG5cbjrpobXpnaLlj5HnlJ_ot7Povaw7XG5cbjrliKTmlq3op6blj5Hot7PovaznmoRzZWd1ZeeahGlkZW50aWZpZXI7XG5cbjrkuLrlr7nlupTnmoRDb250cm9sbGVy55qE5Y-C5pWw6LWL5YC8O1xuXG5zdG9wXG5cbkBlbmR1bWwiLCJ0eXBlIjoicHVtbCIsImlkIjoiaFZVbVQiLCJ1cmwiOiJodHRwczovL2Nkbi5ubGFyay5jb20veXVxdWUvX19wdW1sLzA5YWFkNmNmZTM4NDczMTBhZTQyZmEzNjlmNDEzMjE4LnN2ZyIsImNhcmQiOiJkaWFncmFtIn0=)
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583301132078-63cc3087-07e2-466d-8a90-ffeead054957.png#align=left&display=inline&height=668&name=image.png&originHeight=1522&originWidth=768&size=57335&status=done&style=none&width=337)

### Segue返回
在目标界面的需要触发返回上一界面处填入以下代码。（不可以在Navigation中使用）
```swift
self.presentingViewController!.dismiss(animated: true, completion: nil)
```

第一个参数为动画效果，第二个还不知道。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583303764612-e48b3410-01bc-4a1a-bf97-1260d39d2dc2.png#align=left&display=inline&height=68&name=image.png&originHeight=136&originWidth=804&size=20390&status=done&style=none&width=402)

### Segue回调
回调函数用于接收返回操作时的数据接收。

回调方法：
```swift
//segue回调方法，获取返回参数
@IBAction func backSegue(segue : UIStoryboardSegue){
    if segue.identifier == "backtoMain"{
        //获取返回的控制器
        let backVC = segue.source as! SecondViewController
        lb_one.text = backVC.backstr//获取返回值
    }
}
```
在上面代码中，6行处的backstr为SecondViewController中的一个变量。

将Exit中的回调方法绑定至自己的ViewController：
```swift
@IBAction func backMainVC(_ sender: Any) {
	backstr = "这是P1返回的字符串"
	//如果直接关联到exit，下面的performSegue方法不需要
	performSegue(withIdentifier: "backtoMain", sender: nil)
}
```

右键Exit，把Exit内的回调函数绑定到对应的Button上即可。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583321727647-478b2562-914f-4a6b-a625-343f994df080.png#align=left&display=inline&height=80&name=image.png&originHeight=160&originWidth=416&size=22345&status=done&style=none&width=208)

效果如下：
![QQ20200304-193706-HD.gif](https://cdn.nlark.com/yuque/0/2020/gif/736116/1583321866856-0dec1012-4647-4069-8345-5c0b410c21d2.gif#align=left&display=inline&height=600&name=QQ20200304-193706-HD.gif&originHeight=600&originWidth=302&size=9057961&status=done&style=none&width=302)

