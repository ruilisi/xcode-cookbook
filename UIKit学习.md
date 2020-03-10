# UIKit学习

# 新建工程

新建一个Project，Interface改为Storyboard。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583220834486-ffc4d058-af0e-42f0-9f02-3cc01ee4334d.png#align=left&display=inline&height=526&name=image.png&originHeight=1052&originWidth=1460&size=204524&status=done&style=none&width=730)

建成后的文件目录如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583220903033-4c2f5614-f7a5-49bb-9559-700180cebaf2.png#align=left&display=inline&height=241&name=image.png&originHeight=482&originWidth=540&size=221650&status=done&style=none&width=270)![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583220957002-867400f0-ef00-45ab-bca2-6b4cddf241f6.png#align=left&display=inline&height=245&name=image.png&originHeight=490&originWidth=544&size=227457&status=done&style=none&width=272)
对比先前的SwiftUI，文件结构上没有太大区别，多了一个Main.storyboard文件，少了一个Preview Content文件夹。

ViewController.swift：
```swift
import UIKit

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
}
```

导入的包也从SwiftUI变成了UIKit。

# 绘制界面
参考视频：[https://www.bilibili.com/video/av42035892/?spm_id_from=333.788.videocard.0](https://www.bilibili.com/video/av42035892/?spm_id_from=333.788.videocard.0)

打开Main.storyboard，再点击右上角的加号，拖入一个Button。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583223417626-b4bf6421-b2f7-4855-a025-a6181ded712f.png#align=left&display=inline&height=618&name=image.png&originHeight=1236&originWidth=626&size=27683&status=done&style=none&width=313)

#### 添加事件
按住control拖动Button至ViewController.swift，可以生成一个事件。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583223543993-b6aa6663-565b-4918-ae85-ca6fef7788dc.png#align=left&display=inline&height=165&name=image.png&originHeight=330&originWidth=556&size=47036&status=done&style=none&width=278)
输入Name为事件响应的函数命名，Type为函数的参数类型，Event为这个事件的响应动作，Arguments设置了参数。

生成后的事件代码如下：
```swift
@IBAction func btnClick(_ sender: UIButton) {
    print("Click!")
}
```

启动模拟器，点击按钮。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583223719701-812def42-faee-448e-8823-a0c5323e7c09.png#align=left&display=inline&height=161&name=image.png&originHeight=322&originWidth=538&size=14163&status=done&style=none&width=269)

#### 改变Button的值
由于前面已经将UIButton作为一个sender传入至函数内，所以我们可以对这个函数进行操作，就跟写WPF一样。
```swift
@IBAction func btnClick(_ sender: UIButton) {
        flipCard(text: "Button", button: sender)
    }
    
    func flipCard(text: String, button:UIButton) {
        if button.currentTitle == "Button" {
            button.setTitle("", for: UIControl.State.normal)
            button.backgroundColor = #colorLiteral(red: 0.9529411793, green: 0.6862745285, blue: 0.1333333403, alpha: 1)
        } else {
            button.setTitle(text, for: UIControl.State.normal)
            button.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
        }
    }
```
这里只修改了Button的文字和背景颜色，其他的属性修改大同小异。

#### 添加新的控件
在上面的学习中，我们可以通过拖动的方式在Controller中生成一个Action，但有一个问题，无法跟WPF或Android一样，通过控件ID获取控件进行操作。

通过了解，发现虽然无法直接获取，但是可以通过设置变量来对控件进行操作。

**操作如下：**
同样是按住control对label进行拖动，Connection为Outlet，然后输入个Name就完事儿了。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583225190555-f6d8ea7c-ded2-4621-a57c-c8a7dd3a0492.png#align=left&display=inline&height=147&name=image.png&originHeight=294&originWidth=556&size=40455&status=done&style=none&width=278)

生成后的变量也可以在viewDidLoad函数内调用。

代码如下：
```swift
@IBOutlet weak var lable_song: UILabel!
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        lable_name.text = "好日子"
    }
    
    @IBAction func btnClick(_ sender: UIButton) {
        flipCard(text: "Button", button: sender)
    }
    
    func flipCard(text: String, button:UIButton) {
        if button.currentTitle == "Button" {
            button.setTitle("", for: UIControl.State.normal)
            button.backgroundColor = #colorLiteral(red: 0.9529411793, green: 0.6862745285, blue: 0.1333333403, alpha: 1)
            lable_song.text = "今天是个好日子"
        } else {
            button.setTitle(text, for: UIControl.State.normal)
            button.backgroundColor = #colorLiteral(red: 1, green: 1, blue: 1, alpha: 1)
            lable_song.text = "心想的事儿都能成"
        }
    }
```
**
如果有多个按钮执行相同的事件，将多个按钮的事件绑定在同一个函数上即可，实际操作方式也是通过按住control并拖动即可。

#### 控件数组
在遇到多个Button或相同控件的使用环境时，可以通过创建控件数组来避免代码冗余。

操作和添加Action的方式一样，只不过把Connection改为Outlet Collection即可。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583225813179-b562f0a7-ddd5-49c6-a72d-eb01062dc795.png#align=left&display=inline&height=124&name=image.png&originHeight=248&originWidth=556&size=34930&status=done&style=none&width=278)

生成如下内容：
```swift
@IBOutlet var btns: [UIButton]!
```

需要注意一点，在改变生成的变量名称时，不能直接修改，否则会报错。

右键刚刚关联的Button，可以看见如下属性。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583226038235-a11e9671-81ff-4054-bea1-50cac374a5b4.png#align=left&display=inline&height=431&name=image.png&originHeight=862&originWidth=400&size=78190&status=done&style=none&width=200)

在倒数第二行出，他的变量名称为btns，如果要更改这个名称，需要按住command键，点击需要更改的变量名，然后点击rename进行修改。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583226217021-0eef5b45-2b9b-44fb-bbba-5a06e967bbbd.png#align=left&display=inline&height=201&name=image.png&originHeight=402&originWidth=526&size=228594&status=done&style=none&width=263)

这样修改就不会出问题。

#### 向数组添加内容
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583226335118-ff566350-2a6e-4aca-b481-67ac586538c9.png#align=left&display=inline&height=396&name=image.png&originHeight=792&originWidth=908&size=55513&status=done&style=none&width=454)

同样，按住万能的control，拖动ViewController到想添加的按钮，出现如下画面
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583226396205-490c03c6-cde7-48d7-a89c-89beba6dc157.png#align=left&display=inline&height=106&name=image.png&originHeight=212&originWidth=344&size=30300&status=done&style=none&width=172)
然后点击想要加入的数组就行了。

为三个按钮添加事件：
```swift
@IBAction func btns_click(_ sender: UIButton) {
    var index_btn = buttons.firstIndex(of: sender)
	print("index of:\(index_btn)")
}
```

输入内容：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583226708820-7bad56e5-7306-4dd3-bdda-77307ec14bd6.png#align=left&display=inline&height=110&name=image.png&originHeight=220&originWidth=472&size=14653&status=done&style=none&width=236)

成功输出了对应按钮在数组中的索引。

在新建一个按钮，这个按钮不存在与数组内，如果给新建的按钮也绑定这个事件，那他会输出如下内容：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583226811157-7efb92d1-38a6-47f1-a76d-7ef2e8ae9426.png#align=left&display=inline&height=90&name=image.png&originHeight=180&originWidth=368&size=15006&status=done&style=none&width=184)

因为这个按钮不存在于这个数组，所以会返回nil。

