# CocoaPods

# Hover
将如下代码加入到podfile：
```swift
pod 'CompatibleHover'
```

终端内执行以下命令：
```swift
pod install
```

引入Hover：
```swift
import CompatibleHover
```

使用Hover：
```swift
func setactionButton() {
	// Create Hover's Configuration (all parameters have defaults)
	let configuration = HoverConfiguration(image: UIImage(named: "pokemon"), color: .gradient(top: .white, bottom: .white))
        
	// Create the items to display
	let items = [bindItems()]
        
	// Create an HoverView with the previous configuration & items
	hoverView = HoverView(with: configuration, items: items)
        
	view.addSubview(hoverView)
	// Add to the top of the view hierarchy
	hoverView.translatesAutoresizingMaskIntoConstraints = false
        
	// Apply Constraints
	// Never constrain to the safe area as Hover takes care of that
	NSLayoutConstraint.activate(
		[
			hoverView.topAnchor.constraint(equalTo: view.centerYAnchor),
			hoverView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
			hoverView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
			hoverView.trailingAnchor.constraint(equalTo: view.trailingAnchor)
		]
	)
}

func bindItems() -> HoverItem {
	let itemModel = HoverItem(title: "超稳定模式", image: UIImage(named: VPNDataAccess.isForegroundMode ? "on" : "off")!) {
            
	if(VPNDataAccess.isForegroundMode) {
		VPNDataAccess.isForegroundMode = false
		self.hoverView.items[0].imagec = UIImage(imageLiteralResourceName: "off")
		self.simpleMessage.showNotificationMessage(title: "提示", desc: "超稳定模式已关闭", textColor: EKColor.white)
		}else {
			self.popoverConfirm.show()
			self.popoverConfirm.setCallBack {
                if(VPNDataAccess.isForegroundMode) {
                    self.hoverView.items[0].imagec = UIImage(imageLiteralResourceName: "on")
                }
            }
        }
    }
    return itemModel
}
```

## 定位
`NSLayoutConstraint.activate`用于指定Hover的锚点定位：

- `topAnchor`指定了其顶部锚点，数据类型`NSLayoutYAxisAnchor`（以上例子中指定顶部锚点为Y轴中心）
- `bottomAnchor`指定了底部锚点，数据类型`NSLayoutYAxisAnchor`（以上例子中指定顶部锚点为Y轴底部）
- `leadingAnchor`指定了左边锚点，数据类型`NSLayoutXAxisAnchor`（以上例子中指定顶部锚点为X轴最左侧）
- `trailingAnchor`指定了右边锚点，数据类型`NSLayoutXAxisAnchor`（以上例子中指定顶部锚点为X轴最右侧）

其中，`topAnchor`和`bottomAnchor`绑定Y轴，`leadingAnchor`和`trailingAnchor`指定X轴。

若需要改变图片，可以指向以下代码：
```swift
hoverView.items[0].imagec = UIImage(imageLiteralResourceName: "on")
```

※：此功能可以通过修改Hover库的代码文件进行自定义，同样的，若有对文字的变更需求，可以修改HoverItem文件实现自己想要的功能。

HoverItem.swift：
```swift
import UIKit

// MARK: - HoverItem
public struct HoverItem {
    
    // MARK: Properties
    let title: String?
    var image:UIImage
    public var imagec: UIImage{
        get{
            return image
        }
        set{
            image = newValue
        }
    }
    let onTap: () -> ()
    
    // MARK: Lifecycle
    public init(title: String? = nil, image: UIImage, onTap: @escaping () -> ()) {
        self.title = title
        self.image = image
        self.onTap = onTap
    }
    
}
```

## 事件
事件的代码格式：
```swift
HoverItem(title: "Drop it Anywhere", image: UIImage(named: "anywhere")) 
{ 
    print("Tapped 'Drop it anywhere'") 
}
```
HoverItem事件在声明HoverItem的同时指定。

## 内部参数
### HoverItem字体
关于HoverItem字体的自定义，可以通过对HoverItemView做出修改来实现：
```swift
// MARK: - Condional Constraints
private extension HoverItemView {
    
    func adapt(to orientation: Orientation.X) {
        switch orientation {
        case .leftToRight:
            label.textAlignment = .left
            label.textColor = .black
            add(arrangedViews: button, label)
        case .rightToLeft:
            label.textAlignment = .right
            label.textColor = .black
            add(arrangedViews: label, button)
        }
    }
}
```

`.leftToRight`和`.rightToLeft`分别表示HoverItem出现在右侧和左侧时，label的对应属性，可以随意修改达到不同的需求。

### Hover阴影
Hover的阴影参数存在于UIView+Decorators.swift文件内，代码如下：

```swift
//
//  UIView+Decorators.swift
//  Hover
//
//  Created by Pedro Carrasco on 13/07/2019.
//  Copyright © 2019 Pedro Carrasco. All rights reserved.
//

import UIKit

// MARK: - Gradient
extension UIView {
    
    // MARK: Constant
    private enum GradientConstant {
        static let startPoint = CGPoint(x: 0.5, y: 1.0)
        static let endPoint = CGPoint(x: 0.5, y: 0.0)
        static let locations: [NSNumber] = [0, 1]
    }
    
    // MARK: Functions
    func makeGradientLayer(_ gradientLayer: CAGradientLayer = .init()) -> CAGradientLayer {
        gradientLayer.startPoint = GradientConstant.startPoint
        gradientLayer.endPoint = GradientConstant.endPoint
        gradientLayer.locations = GradientConstant.locations
        layer.insertSublayer(gradientLayer, at: 0)
        return gradientLayer
    }
}

// MARK: - Shadow
extension UIView {
    
    // MARK: Constant
    private enum ShadowConstant {
        static let heightOffset = 0
        static let opacity: Float = 0.3
        static let radius: CGFloat = 6
    }
    
    // MARK: Functions
    func addShadow() {
        layer.shadowColor = UIColor.black.cgColor
        layer.shadowOffset = CGSize(width: 0, height: 5)
        layer.shadowOpacity = ShadowConstant.opacity
        layer.shadowRadius = ShadowConstant.radius
    }
}
```

关于修改阴影参数，只需要对ShadowConstant和addShadow函数进行修改即可。
参数说明：

- `static let opacity: Float `设置了阴影的透明度
- `static let radius: CGFloat `设置了阴影的半径

addShadow函数内对阴影的设置和UIKit控件的设置无异，不多做赘述。

# SwiftEntryKit
SwiftEntryKit是一个非常强大的组件，他可以实现几乎所有的弹窗类型和提示类型，可以考虑日后将这一整套的组件应用于IOS客户端。

将如下代码加入到podfile：
```swift
pod 'SwiftEntryKit'
```

终端内执行以下命令：
```swift
pod install
```

引入SwiftEntryKit：
```swift
import SwiftEntryKit
```

## ToastMessage
ToastMessage常用于通知信息，其实现代码如下：
```swift
import UIKit
import SwiftEntryKit

class SimpleMessage {
    
    func showNotificationMessage(title: String,
                                 desc: String,
                                 textColor: EKColor,
                                 imageName: String? = nil) {
        var attributes = EKAttributes()
        
        attributes = .bottomToast
        attributes.displayMode = .light
        //attributes.entryBackground = .visualEffect(style: .standard)
        attributes.entryBackground = .color(color: EKColor.init(UIColor(red: 231.0 / 255.0, green: 0 / 255.0, blue: 17.0 / 255.0, alpha: 1)))
        attributes.scroll = .edgeCrossingDisabled(swipeable: true)
        attributes.statusBar = .dark
        
        let title = EKProperty.LabelContent(
            text: title,
            style: .init(
                font: UIFont(name: "HelveticaNeue-Medium", size: 16)!,
                color: textColor,
                displayMode: .light
            ),
            accessibilityIdentifier: "title"
        )
        let description = EKProperty.LabelContent(
            text: desc,
            style: .init(
                font: UIFont(name: "HelveticaNeue-Light", size: 14)!,
                color: textColor,
                displayMode: .light
            ),
            accessibilityIdentifier: "description"
        )
        var image: EKProperty.ImageContent?
        if let imageName = imageName {
            image = EKProperty.ImageContent(
                image: UIImage(named: imageName)!.withRenderingMode(.alwaysTemplate),
                displayMode: .light,
                size: CGSize(width: 35, height: 35),
                tint: textColor,
                accessibilityIdentifier: "off"
            )
        }
        let simpleMessage = EKSimpleMessage(
            image: image,
            title: title,
            description: description
        )
        let notificationMessage = EKNotificationMessage(simpleMessage: simpleMessage)
        let contentView = EKNotificationMessageView(with: notificationMessage)
        SwiftEntryKit.display(entry: contentView, using: attributes)
    }
    
}
```

### EKAttributes
`EKAttributes`是SwiftEntryKit的核心，弹窗的外观位置属性都在这里设置。

```swift
attributes = .bottomToast
attributes.displayMode = .light
//attributes.entryBackground = .visualEffect(style: .standard)
attributes.entryBackground = .color(color: EKColor.init(UIColor(red: 231.0 / 255.0, green: 0 / 255.0, blue: 17.0 / 255.0, alpha: 1)))
attributes.scroll = .edgeCrossingDisabled(swipeable: true)
attributes.statusBar = .dark
```

//TODO
第一行属性指定了`attributes`的类型，这里指定的类型为底部提示信息其他的类型还有：

| toast | 弹出提示信息 |
| --- | --- |
| float | 悬浮信息 |
| topFloat | 顶部悬浮信息 |
| bottomFloat | 底部悬浮信息 |
| centerFloat | 中央悬浮信息 |
| bottomToast | 底部弹出信息 |
| topToast | 顶部弹出信息 |
| topNote | 顶部弹窗信息 |
| bottomNote | 底部提示信息 |
| statusBar | 我也不知道，还没用上 |


当然，这些不过是官方已经给出的现成的样式，若无法满足需求也可以自己随心所欲定义。

应该不止这些，其他的有待发现。

`displayMode`：指定了主题风格，这里设定的为`.light`，日间风格，其他的风格还有：

- `inferred`
- `light`
- `dark`

`entryBackground`：指定了背景色，若不指定默认是`.clear`，即透明状态。
`scroll`：指定了是否允许用户滑动关闭提示信息。
`statusBar`：不知道。
`EKSimpleMessage`：简易信息类型，三个参数分别为图像，标题和正文。

最后通过以下三行代码将SimleMessage显示给用户：
```swift
let notificationMessage = EKNotificationMessage(simpleMessage: simpleMessage)
let contentView = EKNotificationMessageView(with: notificationMessage)
SwiftEntryKit.display(entry: contentView, using: attributes)
```

第一第二行还未整明白，但是第三行用于弹出信息。

## CustomView
由于SwiftEntryKit具有很高的自由度，这也使得自定义弹窗成为可能，以下是自定义的过程：
### 自定义一个View：
新建一个View用于弹窗显示，可以在这个View中添加各种需要的控件。
```swift
let width = screenWidth * 0.9
let height = screenHeight * 0.6
confirmView = UIView(frame: CGRect(x: 0, y: 0, width: width, height: height))
confirmView.backgroundColor = .white
confirmView.layer.cornerRadius = 20
confirmView.layer.masksToBounds = true
```

### 自定义EKAttributes：
```swift
self.attributes.position = .center
self.attributes.displayDuration = .infinity
self.attributes.screenInteraction = .dismiss
self.attributes.entryInteraction = .forward
self.attributes.screenBackground = .visualEffect(style: .standard)
self.attributes.shadow = .active(with: .init(color: .black, opacity: 0.3, radius: 10, offset: .zero))
attributes.positionConstraints.size = .init(width: .constant(value: confirmView.frame.width), height: .constant(value: confirmView.frame.height))
```

- `displayDuration`：表示窗口的显示模式，infinity表示这个窗口是永久的。
- `screenInteraction`：表示了点击视窗外部的动作，dismiss表示关闭这个窗口。
- `entryInteraction`：表示用户是否可以和自定义View的内部控件实现交互，默认拒绝。
- `screenBackground`：表示了弹窗显示后的背景显示模式，visualEffect表示模态显示。
- `shadow`：表示了阴影。
- `positionConstraints`.size：表示了尺寸。

### 显示弹窗：
```swift
SwiftEntryKit.display(entry: self.confirmView, using: self.attributes)
```

### 关闭弹窗：
```swift
SwiftEntryKit.dismiss()
```

