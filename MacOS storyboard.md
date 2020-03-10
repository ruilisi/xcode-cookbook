# MacOS storyboard

# 简单布局
storyboard提供了图像化的布局形式，在工程相对简单的情况下，这种布局方式在绘制界面的过程中会让开发变得迅速。
main.storyboard
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583478770027-b8f6be94-0bcb-4f18-a8d3-3fc9ed3f28fd.png#align=left&display=inline&height=320&name=image.png&originHeight=640&originWidth=538&size=58507&status=done&style=none&width=269)

新建的工程会自带一个main.storyboard，包含了**Application Scene**，**Window Control Scene**和**View Controller Scene**三个部分。
## Application Scene
在storyboard看的一目了然，Application Scene用于体现应用程序的菜单栏。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583478927081-d55a50b2-1081-4245-a2e1-7225d4a7d7b7.png#align=left&display=inline&height=290&name=image.png&originHeight=580&originWidth=966&size=68557&status=done&style=none&width=483)

### 结构：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583479001502-58db843b-2009-41cb-9516-259d82687d8c.png#align=left&display=inline&height=339&name=image.png&originHeight=678&originWidth=520&size=56447&status=done&style=none&width=260)

展开后的目录结构如图所示，Menu内包含多个Item，每个Item可以打开新的menu，和Windows的menu无异。

### 事件：
右键一个item即可看见他的Sent Actions，事件的绑定和Button一样。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583479337682-9cb2fb27-2629-45ca-b367-dd1d96af1f51.png#align=left&display=inline&height=247&name=image.png&originHeight=494&originWidth=1560&size=99582&status=done&style=none&width=780)

输出：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583479361803-bf42a2c5-eb1f-4011-98c8-c6382ffa4cf4.png#align=left&display=inline&height=34&name=image.png&originHeight=68&originWidth=292&size=3614&status=done&style=none&width=146)

## Window Controller Scene
Window Controller Scene，是开发的应用程序的视窗，在默认存在的视窗中，通过一个Relationship Segue将Window content指向到后面的View Controller Scene作为显示区域。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583479498629-b807246a-5f7e-4999-8c2c-4c314a540217.png#align=left&display=inline&height=144&name=image.png&originHeight=288&originWidth=734&size=33085&status=done&style=none&width=367)

### 相关属性：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583479748728-d8be2634-8df0-4b35-94f2-ce3dfd71cffd.png#align=left&display=inline&height=256&name=image.png&originHeight=512&originWidth=494&size=52807&status=done&style=none&width=247)

Appearance：

- Title Bar：标题栏；
- Transparent Title Bar：透明标题栏；
- Full Size Content View：**还不知道；**
- Shadow：视窗阴影；
- Textured：**还不知道。**

Controls：
设定左上角的三个关闭，最小化和最大化的启用状态。

Behavior：
**还不知道。**
**
## View Controller Scene
View Controller Scene是布局的主要区域，在这里完成界面的绘制，和IOS应用程序的开发无太大差评。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583480150358-ad6f7b70-6ea6-4470-b34a-0a44a18055f7.png#align=left&display=inline&height=85&name=image.png&originHeight=170&originWidth=530&size=14689&status=done&style=none&width=265)
这里自带一个View，如果需要添加一个Custom View需要做如下操作：
点击右上角+号按钮，搜索Custom View，并拖入目录内想要的位置处。

这时的结构应该如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583480547757-09ca932e-213e-4c8c-8039-00c30e1dfc20.png#align=left&display=inline&height=127&name=image.png&originHeight=254&originWidth=412&size=17911&status=done&style=none&width=206)
其他控件的添加亦然。

### 布局属性
_※这里的布局相当重要，在昨天的学习中有几个地方让我比较蛋疼，先将问题列在这里，日后回头研究。_

- _在一个Vertical Stack View中，加入Horizontal Stack View的时候，其高度无法精确控制；_
- _在向View中添加控件时，总是出现约束错误，原因也无法查明。_

_
添加后的Custom View在storyboard中的显示如下所示：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583480923082-89cb1542-60f5-48f4-bfd5-f04c02683951.png#align=left&display=inline&height=327&name=image.png&originHeight=654&originWidth=1078&size=32739&status=done&style=none&width=539)

如果需要固定在左上位置，则需要做出如下操作：

1. 选中Custom View和初始的View父控件
1. 点击右下角的![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583480986844-432fef8c-93e2-4932-9acd-cb14167025d3.png#align=left&display=inline&height=25&name=image.png&originHeight=50&originWidth=40&size=277&status=done&style=none&width=20)按钮
1. 勾选Leading Edges和Top Edges属性

这个时候会出现报错：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481108027-e4fbbd79-4d96-4ec7-89f6-c9833e1f458b.png#align=left&display=inline&height=175&name=image.png&originHeight=350&originWidth=514&size=11678&status=done&style=none&width=257)

原因是未对Custom View的尺寸做约束。

尺寸的设置有两种方式：

1. 选中需要设置的控件，按下![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481175622-b8cf4591-872b-45f1-be9e-f8b34f173c49.png#align=left&display=inline&height=23&name=image.png&originHeight=46&originWidth=48&size=320&status=done&style=none&width=24)按钮进行设置，此时的尺寸是绝对的。
1. 训中Custom View和View，按下![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481175622-b8cf4591-872b-45f1-be9e-f8b34f173c49.png#align=left&display=inline&height=23&name=image.png&originHeight=46&originWidth=48&size=320&status=done&style=none&width=24)设置尺寸，此时的尺寸是相对的。

这里我使用了方法2，选中对应的属性子项后，我们能在右边属性视窗看到如下界面。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481328604-914c5844-f599-4244-a04e-3eec52269b23.png#align=left&display=inline&height=257&name=image.png&originHeight=514&originWidth=558&size=46480&status=done&style=none&width=279)

**First Item**：当前控件的属性；
**Second Item**：父控件的属性；
**Relation**：对应关系；
**Constant**：不知道；
**Priority**：不知道；
**Multiplier**：倍率。

将长宽倍率都改为0.5：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481497523-a9191e40-71de-409a-8cb9-b9ad9ee9b294.png#align=left&display=inline&height=342&name=image.png&originHeight=684&originWidth=1126&size=38691&status=done&style=none&width=563)

可以看到，具有相对关系的两条边在被选中时会出现阴影提示，现在长宽倍率都为50%。

存在一个问题，Custom View的位置和View的左边以及上边具有一定的间隙。
选中之前设置的位置关系的Item，
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481672538-6014dc1e-e01c-47d3-a6b9-0856542e9346.png#align=left&display=inline&height=40&name=image.png&originHeight=80&originWidth=440&size=9314&status=done&style=none&width=220)

在Constant一栏内填入0。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481691046-57849259-16d7-48ce-abb8-1e212683dff0.png#align=left&display=inline&height=268&name=image.png&originHeight=536&originWidth=542&size=47355&status=done&style=none&width=271)

此时的Custom View完美依靠在View的边缘
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583481705645-d2a4a1bd-1c02-41ee-95a0-10278a2017bf.png#align=left&display=inline&height=348&name=image.png&originHeight=696&originWidth=1070&size=30091&status=done&style=none&width=535)

从前面的操作我们知道，父子控件的长宽比可以通过Multiplier属性设置倍率，在设置Custom View的左边和上边距离时，在对应的属性窗口内也具有这个属性。

但是我发现，在现在Custom View的这个位置，无论我对Multiplier的值进行怎样的设定，都起不了作用，所以我换到了右下角。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583482589335-1e6fc70d-a937-44a3-bd61-8f08a665a4b9.png#align=left&display=inline&height=347&name=image.png&originHeight=694&originWidth=1064&size=35940&status=done&style=none&width=532)![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583482597699-7087e55f-2931-4d8b-b541-bbd2308538c5.png#align=left&display=inline&height=99&name=image.png&originHeight=198&originWidth=674&size=26055&status=done&style=none&width=337)

这个时候却都可以了。

原因不知道，或许和坐标系有关系。

通过这些相对布局的设定，就可以做到仅仅改变父控件的尺寸就可以对不同的屏幕尺寸进行适配。

### 对比区别
#### 绝对布局的情况：
为了可以看到效果，将Custom View删除，添加一个Button，设置右边边距和底边边距为15。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583484322096-4f638a38-ccb0-4a53-886b-f997ad1eb87b.png#align=left&display=inline&height=141&name=image.png&originHeight=282&originWidth=702&size=25802&status=done&style=none&width=351)

窗口化：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583484369968-62e04857-6c41-475b-8c02-cee565e746cd.png#align=left&display=inline&height=292&name=image.png&originHeight=584&originWidth=960&size=32500&status=done&style=none&width=480)

拉伸后：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583484744014-29291804-2c98-44fc-ac43-da57ec2dd2af.png#align=left&display=inline&height=174&name=image.png&originHeight=348&originWidth=1616&size=35515&status=done&style=none&width=808)

可以看见，并没有因为窗口的变化而调整距离。

#### 相对布局的情况：
通过设置Multiplier的数据为1.08按比例进行调整：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583484451016-00854966-f74b-4109-b9fd-48332c211f40.png#align=left&display=inline&height=146&name=image.png&originHeight=292&originWidth=676&size=26627&status=done&style=none&width=338)

窗口化：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583484493411-6b5b1c8d-6419-4d0b-a5cb-90b7229401f7.png#align=left&display=inline&height=292&name=image.png&originHeight=584&originWidth=960&size=32244&status=done&style=none&width=480)

拉伸后：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583484674912-3f4ce2d5-ca3d-42b3-8c18-b15f1c64d66b.png#align=left&display=inline&height=484&name=image.png&originHeight=968&originWidth=292&size=21777&status=done&style=none&width=146)

显然，当全屏化的时候，Button的位置因为主窗体的尺寸发生了改变，其对于底边和右边的边距也发生了改变。
