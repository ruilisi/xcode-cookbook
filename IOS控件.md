# IOS控件

# UITextField常用属性
```swift
@IBOutlet weak var tf_account: UITextField!
@IBOutlet weak var tf_pwd: UITextField!
    
override func viewDidLoad() {
    super.viewDidLoad()
    // Do any additional setup after loading the view.
    tf_account.placeholder = "请输入账号"//提示文本
    tf_pwd.placeholder = "请输入密码"//提示文本
    tf_account.adjustsFontSizeToFitWidth=true  //当文字超出文本框宽度时，自动调整文字大小
    tf_account.minimumFontSize=9  //最小可缩小的字号
    tf_account.clearButtonMode = .whileEditing  //编辑时出现清除按钮
    tf_account.becomeFirstResponder()//在界面打开时获取焦点，弹出键盘
    tf_account.resignFirstResponder()//失去焦点时，收起键盘
    tf_account.returnKeyType = UIReturnKeyType.done//修改键盘的回车作用
    tf_account.textColor = UIColor.cyan//输入/显示文本字体的颜色
    tf_account.font = UIFont.systemFont(ofSize: 14)//设置文本的字体大小
    tf_account.backgroundColor = UIColor.black//修改背景色
    tf_account.autocorrectionType = UITextAutocorrectionType.no//自动纠错
    
    tf_pwd.clearButtonMode = .whileEditing  //编辑时出现清除按钮
    //.unlessEditing  //编辑时不出现，编辑后才出现清除按钮
    //.always  //一直显示清除按钮
    tf_pwd.isSecureTextEntry = true//密码
    tf_pwd.keyboardType = UIKeyboardType.numberPad//数字键盘
        
}
```

## 清除按钮的参数：
**.whileEditing：**编辑时出现清除按钮；
**.unlessEditing：**编辑时不出现，编辑后才出现清除按钮；
**.always：**一直显示清除按钮。

## 键盘类型
```swift
tf_pwd.keyboardType = UIKeyboardType.numberPad//数字键盘
```

### 键盘的几种枚举：
**Default：**系统默认的虚拟键盘；
**ASCII Capable：**显示英文字母的虚拟键盘；
**Numbers and Punctuation：**显示数字和标点的虚拟键盘；
**URL**：显示便于输入url网址的虚拟键盘；
**Number Pad：**显示便于输入数字的虚拟键盘；
**Phone Pad：**显示便于拨号呼叫的虚拟键盘；
**Name Phone Pad：**显示便于聊天拨号的虚拟键盘；
**Email Address：**显示便于输入Email的虚拟键盘；
**Decimal Pad：**显示用于输入数字和小数点的虚拟键盘；
**Twitter：**显示方便些Twitter的虚拟键盘；
**Web Search：**显示便于在网页上书写的虚拟键盘。

## 设置键盘回车的功能
```swift
tf_account.returnKeyType = UIReturnKeyType.done//修改键盘的回车作用
```

### 修改键盘的回车功能：
**UIReturnKeyType.done：**表示完成输入
**UIReturnKeyType.go：**表示完成输入，同时会跳到另一页
**UIReturnKeyType.search：**表示搜索
**UIReturnKeyType.join：**表示注册用户或添加数据
**UIReturnKeyType.next：**表示继续下一步
**UIReturnKeyType.send：**表示发送

### 实现回车功能：
```swift
class ViewController: UIViewController,UITextFieldDelegate {
    @IBOutlet weak var tf_account: UITextField!
    override func viewDidLoad() {
        super.viewDidLoad()
        
        tf_account.returnKeyType = UIReturnKeyType.done//修改键盘的回车作用
        tf_account.delegate = self//输入框代理
    }
    
	func textFieldShouldReturn(_ textField: UITextField) -> Bool {
    	//收起键盘
   		//textField.resignFirstResponder()
        //获取密码输入框焦点
        tf_pwd.becomeFirstResponder()
    	//打印出文本框中的值
    	print(textField.text)
    	return true;
	}
}
```
通过继承UITextFieldDelegate实现自定义回车事件。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583569885274-4f33cf23-035c-4a09-ab9f-823ee9a07534.png#align=left&display=inline&height=57&name=image.png&originHeight=114&originWidth=324&size=4501&status=done&style=none&width=162)

## 自动纠错
```swift
tf_account.autocorrectionType = UITextAutocorrectionType.no//自动纠错
```

### 纠错枚举：
**default：**默认
**no：**不纠错
**yes：**纠错

参考：
[https://www.jianshu.com/p/63bdeca39ddf](https://www.jianshu.com/p/63bdeca39ddf)
[https://developer.apple.com/documentation/uikit/uitextfield#topics](https://developer.apple.com/documentation/uikit/uitextfield#topics)

# 通过后台代码绘制IOS控件

## 常规按钮
```swift
class ViewController: UIViewController,UITextFieldDelegate {

    var btn_login = UIButton()
    
    let screenSize = UIScreen.main.bounds
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.

        let screenWidth = screenSize.width
        //let screenHeight = screenSize.height
        
        let btn_one = UIButton()
        btn_one.frame = CGRect(x:screenWidth/2-40, y:84, width:80, height:30)
        btn_one.setTitle("FirstBtn", for: .normal)
        btn_one.setTitleColor(UIColor.red, for: .normal)
        //无参数事件
        //btn_one.addTarget(self, action: #selector(buttonClick(button:)), for: .touchUpInside)
        //有参数事件
        btn_one.addTarget(self, action: #selector(buttonClick(button:)), for: .touchUpInside)
        self.view.addSubview(btn_one)
    }
    
    //无参数点击事件
    @objc func buttonClick(){
          print("点击了button")
    }
    //带参数点击事件
    @objc func buttonClick(button:UIButton ){
    button.isSelected = !button.isSelected
    if button.isSelected {
            button.setTitle("Selected", for: .normal)
    }else{
            button.setTitle("NoSelected", for: .normal)
        }
    }

}
```

**.frame：**设置控件的在屏幕中的位置，控件的宽度和控件的高度。
**.setTitle：**设置控件的文字
**.setTitleColor：**设置控件的颜色
**.setTarget：** 设置控件的事件（.touchUpInside表示这是一个点击事件，其他的操作参考UIControl文件）

通过 `self.view.addSubview(btn_one)` 将控件添加至界面。

## 为按钮设置图片
```swift
override func viewDidLoad() {
	super.viewDidLoad()
        
	let screenWidth = screenSize.width
	let button1 = UIButton(frame:CGRect(x:screenWidth/2-40, y:84, width:80, height:80))
	self.view.addSubview(button1)
	button1.setTitle("包子", for: .normal)
	button1.setTitleColor(UIColor.black, for: .normal)
	//button1.setImage(UIImage(named:"Ava"), for: .normal)
	button1.setBackgroundImage(UIImage(named:"Ava"), for: .normal)
	//button1.backgroundColor = UIColor.red
	// 上左下右 根据自己图片和文字布局自行调整参数设置
	//button1.imageEdgeInsets = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
	button1.titleEdgeInsets = UIEdgeInsets(top: 0, left: 0, bottom: 0, right: 0)
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583576030226-79d304fa-1cb9-4062-b082-6a9d525c3bd4.png#align=left&display=inline&height=58&name=image.png&originHeight=116&originWidth=132&size=8295&status=done&style=none&width=66)

这里需要注意，如果使用`setImage`设置图片，图片将把文字遮挡，使用`setBackgroundImage`则不会。

