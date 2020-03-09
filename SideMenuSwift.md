# SideMenuSwift

侧滑菜单需要通过SideMenuSwift实现。
[https://github.com/kukushi/SideMenu](https://github.com/kukushi/SideMenu)

# 安装pod
新建工程后，cd到自己的工程内，执行以下命令：
```swift
pod init
pod install
```

# 添加pod第三方库
修改Podfile：
```swift
target 'slideMenu' do
  # Comment the next line if you don't want to use dynamic frameworks
  # Pods for slideMenu
  pod 'SideMenuSwift'
end
```

执行以下命令：
```swift
pod install
```

# 修改storyboard
## 修改第一个ViewController
对storyboard内自带的View Controller做出如下改动：

- 将Class更改为SideMenuController
- 将Module改为SideMenuSwift

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583664559759-46cee136-5e8d-4167-989e-db9392116182.png#align=left&display=inline&height=198&name=image.png&originHeight=396&originWidth=554&size=33252&status=done&style=none&width=277)

## 添加Menu View
新增一个ViewController，做出如下修改：

- 将Class改为MenuViewController

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583667288772-c7ed7556-b70e-42fc-84ab-df9fce8d997d.png#align=left&display=inline&height=153&name=image.png&originHeight=306&originWidth=546&size=28188&status=done&style=none&width=273)

在MenuViewController中，对侧边菜单的内容进行自定义。

### 添加Segue
在SideMenuController和MenuController之间添加一个Custom Segue，

- 将Identifier改为SideMenu.Menu，
- 将Class改为SideMenuSegue
- 将Module改为SideMenuSwift

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583667389423-409d6a19-3223-46d8-a186-f8a611637cfe.png#align=left&display=inline&height=116&name=image.png&originHeight=232&originWidth=524&size=24206&status=done&style=none&width=262)

## 添加Content View
### 第一步
添加一个NavigationController，做出如下变动：

- 将Class改为NavigationController

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583667547655-4b890918-8901-4312-bd3a-70ecb5a354ca.png#align=left&display=inline&height=113&name=image.png&originHeight=226&originWidth=530&size=23420&status=done&style=none&width=265)

NavigationController：
```swift
import UIKit

class NavigationController: UINavigationController {

    open override var childForStatusBarHidden: UIViewController? {
        return self.topViewController
    }

    open override var childForStatusBarStyle: UIViewController? {
        return self.topViewController
    }
}
```

### 第二部
删除自带的TableViewController，添加一个ViewController，并添加一个Bar Button Item。
将这个ViewController的Class更改为CotentViewController。

CotentViewController：
```swift
import UIKit
import SideMenuSwift

class CotentViewController: UIViewController {
    
    override func viewDidLoad() {
        
        super.viewDidLoad()
        title = "Preferences"

        setNeedsStatusBarAppearanceUpdate()
    }
    
    @IBAction func menuButtonClicked(_ sender: Any) {
        sideMenuController?.revealMenu()
    }
}
```

在CotentViewController中，可以自由对主页的内容进行自定义

完成后的playboard：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583667858505-8986296f-ffe9-4981-885a-7c04fe079b27.png#align=left&display=inline&height=795&name=image.png&originHeight=1590&originWidth=1850&size=168849&status=done&style=none&width=925)


设置一些初始属性，打开AppDelegate做出如下修改：
```swift
import UIKit
import SideMenuSwift
@UIApplicationMain
class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?

    func application(_ application: UIApplication,
                     didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        #if DEBUG
        var arguments = ProcessInfo.processInfo.arguments
        arguments.removeFirst()
        setupTestingEnvironment(with: arguments)
        #endif

        configureSideMenu()
        return true
    }

    private func configureSideMenu() {
        SideMenuController.preferences.basic.menuWidth = 240
        SideMenuController.preferences.basic.defaultCacheKey = "0"
    }
}

#if DEBUG
extension AppDelegate {
    private func setupTestingEnvironment(with arguments: [String]) {
        if arguments.contains("SwitchToRight") {
            SideMenuController.preferences.basic.direction = .right
        }
    }
}
#endif
```

效果：

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583667982213-9e5e72cf-8f8c-42b1-9498-6369b7756d7d.png#align=left&display=inline&height=626&name=image.png&originHeight=1514&originWidth=764&size=67550&status=done&style=none&width=316)![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583667990861-62dc285c-2787-4e69-8b92-9e20bfea0f95.png#align=left&display=inline&height=628&name=image.png&originHeight=1514&originWidth=764&size=82909&status=done&style=none&width=317)


其他的自定义属性：
```swift
SideMenuController.preferences.basic.menuWidth = 240
SideMenuController.preferences.basic.statusBarBehavior = .hideOnMenu
SideMenuController.preferences.basic.position = .below
SideMenuController.preferences.basic.direction = .left
SideMenuController.preferences.basic.enablePanGesture = true
SideMenuController.preferences.basic.supportedOrientations = .portrait
SideMenuController.preferences.basic.shouldRespectLanguageDirection = true
```



Caching the Content

```swift
// Cache the view controllers somewhere in your code
sideMenuController?.cache(viewController: secondViewController, with: "second")
sideMenuController?.cache(viewController: thirdViewController, with: "third")
// Switch to it when needed
sideMenuController?.setContentViewController(with: "second")
```
**这里还没搞明白**

