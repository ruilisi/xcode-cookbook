# IOS组件

# DropDown
## 前置工作

配置Podfile:
```swift
pod 'DropDown'
```

安装组件
```shell
pod install
```

导入DropDown
```swift
import DropDown
```

## 基础使用
```swift
let dropDown = DropDown()

// The view to which the drop down will appear on
dropDown.anchorView = view // UIView or UIBarButtonItem

// The list of items to display. Can be changed dynamically
dropDown.dataSource = ["Car", "Motorcycle", "Truck"]
```

- anchorView：用于你所需要打开DropDown的View
- DataSource：数据源，可以动态获取

### 其他属性
```swift
// Action triggered on selection
dropDown.selectionAction = { [unowned self] (index: Int, item: String) in
  print("Selected item: \(item) at index: \(index)")
}

// Will set a custom width instead of the anchor view width
dropDownLeft.width = 200

dropDownLeft.backgroundColor = #colorLiteral(red: 1.0, green: 1.0, blue: 1.0, alpha: 1.0)
dropDownLeft.shadowColor = #colorLiteral(red: 0.4470588235, green: 0.6352941176, blue: 0.8941176471, alpha: 1)
        
```

- selectionAction：选中后的事件
- width：宽度
- backgroundColor：背景色
- shadowColor：阴影色，默认偏移为0，0

### 显示和隐藏
```swift
dropDown.show()
dropDown.hide()
```

- show()：用于显示DropDown
- hide()：用于隐藏DropDown

※注：DropDown的锚点与用于触发DropDown的View相同。

## 自定义Cell
若需要实现在DropDown内显示图片等额外信息，需要对Cell进行自定义。

### 新建View
依次点击：File->New File->View，创建一个新的View，并删除自带的View，使用Table Cell。
加入想要的控件。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583843566496-843450bb-8b83-4a30-a1c5-1701903eab60.png#align=left&display=inline&height=136&name=image.png&originHeight=272&originWidth=674&size=20684&status=done&style=none&width=337)

#### 新建MyCell
新建一个MyCell的swift文件，继承自DropDownCell，并将上面添加的ImageView加入到新建的MyCell：
```swift
import UIKit
import DropDown

class MyCell: DropDownCell {
    @IBOutlet weak var logoImageView: UIImageView!
}
```

#### 绑定Label
对于Label的绑定，显得比较特殊，这里不可以将其拖放至MyCell，而应该与所继承的父类DropDownCell内的`@IBOutlet open weak var optionLabel: UILabel!`相绑定。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583843775205-97fb7568-aca3-446e-8ae9-e233991b3a11.png#align=left&display=inline&height=38&name=image.png&originHeight=76&originWidth=462&size=18201&status=done&style=none&width=231)

### 调用
```swift
linesDropDownView.cellNib = UINib(nibName: "MyCell", bundle: nil)
        
linesDropDownView.customCellConfiguration = { (_: Int, item: String, cell: DropDownCell) -> Void in
guard let cell = cell as? MyCell else { return }
            
	// Setup your custom UI components
            
	let url = URL(string: "https://res.paiyou.co/OralFlag\(item).png")
	do {
		cell.logoImageView.kf.setImage(
			with: url,
			options: [
				.transition(.fade(1))
			])
	}catch let error as NSError {
		print(error)
	}
}
```

※特别注意：若不为继承的DropDownCell内的`optionLabel`绑定一个Label，则会引发运行错误。

实际效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583843913800-b9888a4a-06a8-4cc4-80e6-cbb4979237ac.png#align=left&display=inline&height=757&name=image.png&originHeight=1514&originWidth=764&size=122936&status=done&style=none&width=382)

---


# 加载动画
将载入动画添加至View：
```swift
let loadingAnimationView = AnimationView()

loadingAnimationView.frame = CGRect(x: screenWidth * 0.11, y: screenHeight * 0.85 - 75, width: screenWidth * 0.78, height: 200)
loadingAnimationView.animation = Animation.named("progress-bar")
loadingAnimationView.backgroundColor = .clear
loadingAnimationView.loopMode = .loop
view.addSubview(loadingAnimationView)
```

这里所使用的的progress-bar为载入动画的json文件。

- backgroundColor：表示背景色，默认clear，透明
- loopMode：循环播放

调用：
```swift
private func updateButtonLoadingView(_ play: Bool) {
	play ? self.loadingAnimationView.play() : self.loadingAnimationView.stop()
	self.loadingAnimationView.isHidden = !play
}
```

- play()：播放该载入特效
- stop()：停止播放
- isHidden()：设置隐藏

