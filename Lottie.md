# Lottie

Lottie可以用于实现动画效果，动画基本效果为一个json文件，这个文件可以通过AE导出，在通过Lottie即可实现。
Cocoa:[http://cocoadocs.org/docsets/lottie-ios/2.0.5/](http://cocoadocs.org/docsets/lottie-ios/2.0.5/)
gitub:[https://github.com/airbnb/lottie-ios](https://github.com/airbnb/lottie-ios)
website:[http://airbnb.io/lottie/#/ios](http://airbnb.io/lottie/#/ios)
# 安装Lottie
将如下代码加入到podfile：
```swift
pod 'lottie-ios'
```


终端内执行以下命令：
```swift
pod install
```

引入Hover：
```swift
import Lottie
```

# AnimationView
AnimationView可以用于显示json动画效果，是Lottie最基础的使用方式，可以达到如图所示的效果：
![oq256-xymbz.gif](https://cdn.nlark.com/yuque/0/2020/gif/736116/1584506864929-7004bfcf-c94c-4c3e-b9e3-346ac811dea4.gif#align=left&display=inline&height=960&name=oq256-xymbz.gif&originHeight=960&originWidth=544&size=5521889&status=done&style=none&width=544)

## 如何使用
🌰代码：
```swift
let loadingAnimationView = AnimationView()
func animLoad() {
	loadingAnimationView.frame = CGRect(x: screenWidth * 0.11, y: screenHeight * 0.85 - 75, width: screenWidth * 0.78, height: 200)
	loadingAnimationView.animation = Animation.named("progress-bar")
    loadingAnimationView.backgroundColor = .clear
    loadingAnimationView.loopMode = .loop
    loadingAnimationView.animationSpeed = 0.5
    view.addSubview(loadingAnimationView)
    loadingAnimationView.play()
}
```

属性说明：

- `animation`：指定`AnimationView`需要播放的json动画文件名，这个json文件需要在包含在项目内，其数据类型为`Animation`。
- `backgroundColor`：用于指定背景色，数据类型为`UIColor`。
- `loopMode`：用于指定播放模式，数据类型为`LottieLoopMode`
  - `playOnce`：播放一次。
  - `loop`：循环播放。
  - `autoReverse`：往复播放一遍。
  - ``repeat`(Float)`：循环指定次数。
  - `repeatBackwards(Float)`：往复播放指定次数。
- `animationSpeed`：播放的速度。

## 其他属性
### Respect Animation Frame Rate
```swift
var AnimationView.respectAnimationFrameRate: Bool { get set }
```

- 若设置为true，将以json编码文件的速率来播放；
- 若为false，将以设备的帧率来播放。

### [Is Animation Playing](http://airbnb.io/lottie/#/ios?id=is-animation-playing)
```swift
var AnimationView.isAnimationPlaying: Bool { get set }
```
用于判断当前动画是否在播放，若为`true`则为正在播放，`false`则为没在播放。（没搞懂为什么要set，停止播放直接pause或者stop不就达到效果了吗？）

### [Background Behavior](http://airbnb.io/lottie/#/ios?id=background-behavior)
```swift
var AnimationView.backgroundBehavior: LottieBackgroundBehavior { get set }
```
设置动画在后台时的状态（仅限于IOS），默认为`.pause`，包含以下属性：

- `stop`：停止
- `pause`：暂停
- `pauseAndRestore`：暂停，但在回到界面时继续

### Current Progress
```swift
var AnimationView.currentProgress: AnimationProgressTime { get set }
```
用于设置和读取当前的进度。

### Current Time
```swift
var AnimationView.currentTime: TimeInterval { get set }
```
用于设置和读取当前的时间节点。

### Current Frame
```swift
var AnimationView.currentFrame: AnimationFrameTime { get set }
```
用于读取和设置当前帧。

### Force Display Update
```swift
func AnimationView.forceDisplayUpdate()
```
强制重新绘制内容。

## play()播放动画
通过上面的例子不难看出，在设置完`AnimationView`的对应属性后，只需调用`play()`即可实现动画播放，值得注意的是，`play()`并非中动画的开头播放至结尾，而是从当前状态开始播放，直至结尾，例如当你调用`pause()`函数暂停动画，在通过`play()`函数开始，动画会从暂停的地方继续播放，而不是重新播放。

play函数代码：
```swift
public func play(completion: LottieCompletionBlock? = nil) {
	guard let animation = animation else {
      return
    }
    
    /// Build a context for the animation.
    let context = AnimationContext(playFrom: CGFloat(animation.startFrame),
                                   playTo: CGFloat(animation.endFrame),
                                   closure: completion)
    removeCurrentAnimation()
    addNewAnimationForContext(context)
}
```

官方对于该参数的说明：
_An optional completion closure to be called when the animation completes playing._

可以理解到，`completion`参数作为动画播放结束后执行的一个必报函数，可以在动画播放结束后进行其他操作。

### 其他重构函数
其他的重构函数有：
```swift
public func play(fromProgress: AnimationProgressTime? = nil,
                   toProgress: AnimationProgressTime,
                   loopMode: LottieLoopMode? = nil,
                   completion: LottieCompletionBlock? = nil) 

public func play(fromFrame: AnimationFrameTime? = nil,
                   toFrame: AnimationFrameTime,
                   loopMode: LottieLoopMode? = nil,
                   completion: LottieCompletionBlock? = nil) 

public func play(fromMarker: String? = nil,
                   toMarker: String,
                   loopMode: LottieLoopMode? = nil,
                   completion: LottieCompletionBlock? = nil) 
```

#### 参数说明
这三个重写的play函数都具有四个参数，前两个各有不同的作用，后两个作用统一：

- `loopMode`：表示播放的模式。
- `completion`：动画播放完成后的回调闭包函数。

在第一个函数中的参数说明如下：

- `fromProgress`：动画开始的进度，若为nil则从0开始；
- `toProgress`：结束进度；

第二个函数中的参数说明如下：

- `fromFrame`：动画开始的帧，若为nil则从当前帧开始；
- `toFrame`：结束帧；

第三个函数中的参数说明如下：

- `fromMarker`：动画开始的标记，若为nil则从当前标记开始；
- `toMarker`：结束标记。

其他播放相关的函数：

- `pause()`：暂停动画；
- `stop()`：停止动画。

## Animation
Animation用于存放json动画，可以通过Animation对json动画的播放时段，尺寸等参数进行设置。

参数：

- `version`：json动画的版本，数据类型 `String`；
- `type`：动画的类型，有3D和2D，数据类型 `CoordinateSpace`；
- `startFrame`：动画的起始时间节点，数据类型 `AnimationFrameTime`；
- `endFrame`：动画的结束时间节点，数据类型 `AnimationFrameTime`；
- `framerate`：动画的帧率，数据类型 `Double`；
- `width`：宽度，数据类型 `Int`；
- `height`：高度，数据类型 `Int`；
- `layers`：图层数组，数据类型 `[LayerModel]`；
- `glyphs`：字形的数组，数据类型 `[Glyph]?`；
- `fonts`：字体数组，数据类型 `FontList?`
- `assetLibrary`：资源库，`AssetLibrary?`

# Animated Button
AnimationView不具备点击功能，因此如果有动画按钮的需求，AnimationView就很难满足需求，但可以通过Lottie库提供的AnimatedButton实现该功能。
AnimatedButton可以允许我们使用一个json动画作为按钮的显示动效。

🌰举个例子：
![1894-submit-check-mark.gif](https://cdn.nlark.com/yuque/0/2020/gif/736116/1584508223785-aa080fc9-4e9b-45b7-9868-e235976c1770.gif#align=left&display=inline&height=286&name=1894-submit-check-mark.gif&originHeight=640&originWidth=640&size=27662&status=done&style=none&width=286)
## 如何使用
🌰代码：
```swift
var anButton = AnimatedButton()
let anm = Animation.named("botton")
anButton.animation = anm
anButton.translatesAutoresizingMaskIntoConstraints = false//自适应布局
anButton.clipsToBounds = false//裁剪超出父视图的部分
anButton.setPlayRange(fromProgress: 0, toProgress: 1, event: .touchUpInside)
anButton.addTarget(self, action: #selector(starClick(sender:)), for: .touchUpInside)
btView.addSubview(anButton)
```

属性说明：

- `animation`：指定`AnimatedButton`需要播放的json动画文件名，这个json文件需要在包含在项目内，其数据类型为`Animation`。
- `setPlayRange`：指定`AnimatedButton`所播放的动画的范围：
  - fromProgress：动画开始的进度，默认为0；
  - toProgress：结束的进度；
  - event：触发的事件。

`setPlayRange`除了可以通过进度指定播放的范围，也可以通过标记实现相同的功能。

可以看到，`AnimatedButton`和`AnimationView`一样，需要一个`Animation`作为他的动画。

# Animated Switch
IOS的UISwitch几乎不可自定义，AnimatedSwitch可以通过Lottie json动画文件实现各类效果的Switch。

🌰举个例子：
![440-stop-go-radio-button.gif](https://cdn.nlark.com/yuque/0/2020/gif/736116/1584510707622-9c2b4b15-e3a0-4523-a218-821d038cac41.gif#align=left&display=inline&height=271&name=440-stop-go-radio-button.gif&originHeight=640&originWidth=640&size=49554&status=done&style=none&width=271)

## 如何使用
🌰代码：
```swift
let anSwitch = AnimatedSwitch()
anSwitch.animation = Animation.named("mineswitch")
anSwitch.setIsOn(true, animated: true)
anSwitch.setProgressForState(fromProgress: 0.44, toProgress: 0.73, forOnState: true)
anSwitch.setProgressForState(fromProgress: 0, toProgress: 0.32, forOnState: false)
```

通过设置`setProgressForState`可以实现`switch`效果的自定义。
参数：

- `fromProgress`：起始进度；
- `toProgress`：结束进度；
- `forOnState`：以上时段的动画在哪个状态执行。


