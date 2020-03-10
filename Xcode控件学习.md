# Xcode控件学习

# 列表的实现
列表通过List组件实现，代码如下：
```swift
List(0 ..< 5) { item in
       Text("hello")
               .font(.title)
}
```
_——使用__`List`__组件可以快速的创建滑动列表，不需要设置代理，不需要实现协议方法就达到类似于UIKit中UITableView的效果。_

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583133835526-96aebfcc-4cf8-4dbd-9c70-ada7fb4ae4b5.png#align=left&display=inline&height=538&name=image.png&originHeight=884&originWidth=444&size=30722&status=done&style=none&width=270)

```swift
struct LandmarkRow : View {
    var landmark: Landmark
    var body: some View {
        HStack {
            landmark.image(forSize: 50)
            Text(landmark.name)
        }
    }
}
```

# 数据样本
之前学习了如何将信息通过hard code的方式填入到自定义控件内，这次开始尝试进行数据绑定。

官方给的数据图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583139596950-47ef574c-eddf-4c83-88cc-02963279440a.png#align=left&display=inline&height=244&name=image.png&originHeight=488&originWidth=910&size=46029&status=done&style=none&width=455)

显然，这次要完成的内容包含了数据详情的跳转查看功能。

## 基本结构
下载官方的入门项目，其目录如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583139671729-54b7a42d-3a08-4c01-92fa-1a704f9d48be.png#align=left&display=inline&height=455&name=image.png&originHeight=910&originWidth=600&size=666506&status=done&style=none&width=300)

## 数据来源
可以看到其目录下有一个名为Models的文件夹，这里包含了两个文件，Data用于数据的载入，Landmark则为数据结构。

打开资源文件，可以看到包含一个json文件，这个json文件就是需要绑定的数据来源。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583139808238-c217ab9e-ed8c-4f6c-a2a5-01a5c4fed8fc.png#align=left&display=inline&height=862&name=image.png&originHeight=1724&originWidth=2880&size=518508&status=done&style=none&width=1440)

## 视图
打开LandmarkDetail.swift文件，这个文件描绘了数据细节的一个详情界面，其界面的Swift UI代码如下：
```swift
struct LandmarkDetail: View {
    var body: some View {
        VStack {
            MapView()
                .edgesIgnoringSafeArea(.top)
                .frame(height: 300)
            CircleImage()
                .offset(x: 0, y: -130)
                .padding(.bottom, -130)
            VStack(alignment: .leading) {
                Text("Turtle Rock")
                    .font(.title)
                HStack(alignment: .top) {
                    Text("Joshua Tree National Park")
                        .font(.subheadline)
                    Spacer()
                    Text("California")
                        .font(.subheadline)
                }
            }
            .padding()
            Spacer()
        }
    }
}
```

MapView()：一个用于显示地图的自定义组件。
CircleImage()：自定义圆形图片。
剩下的就和昨天内容大同小异了。

预览效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583140149061-8f8b99b3-1f75-4a10-95de-63496f70fb66.png#align=left&display=inline&height=761&name=image.png&originHeight=1522&originWidth=768&size=524392&status=done&style=none&width=384)

以上为入门项目给出的内容，接下来开始制作List界面。

## 新建行视图界面
创建一个名为的新SwiftUI视图。`LandmarkRow.swift`
新建的时候需要注意，如果需要新建的文件为SwiftUI文件，则选择如下：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583140281938-d0e5f9ba-9285-4bfd-80a2-26c290b17a24.png#align=left&display=inline&height=526&name=image.png&originHeight=1052&originWidth=1460&size=514961&status=done&style=none&width=730)
然后填入文件的名称即可。

新建的视图代码如下：
```swift
import SwiftUI

struct LandmarkRow: View {
    
    var body: some View {
        Text(/*@START_MENU_TOKEN@*/"Hello, World!"/*@END_MENU_TOKEN@*/)
    }
}

struct LandmarkRow_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkRow()
    }
}
```

在LandmarkRow内加入以下代码：
```swift
var landmark: Landmark
```

出现如下报错：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583140437400-9d1d6270-6055-4a3a-a008-bc7ba7b65194.png#align=left&display=inline&height=128&name=image.png&originHeight=256&originWidth=640&size=33642&status=done&style=none&width=320)

显然，我们缺失了参数，只要在这里填入一个Landmark参数即可，在括号呢加入如下参数：
`landmark:landmarkData[0]`
这里的参数为上面提到的json文件内的第一个数据。

发现：
现在需要将Text加入到HStack容器内，除了正常的打代码，Xcode还提供了一种相对方便的方法，按住command点击Text，可以快速将对应内容加入到容器内。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583140784039-6f609ad1-da31-4161-83de-91416667d7fa.png#align=left&display=inline&height=440&name=image.png&originHeight=880&originWidth=628&size=417071&status=done&style=none&width=314)

### 数据读取
#### 文本
现在试着调用传入的数据，将Text内的Hello,World!替换为`landmark.name`，得到如下结果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583140926872-4027aad6-8cb1-4e7f-9ad8-c762242207dc.png#align=left&display=inline&height=552&name=image.png&originHeight=1104&originWidth=728&size=30549&status=done&style=none&width=364)
这说明数据已经顺利被读取到了。

#### 图片
加入图片的读取：
```swift
var body: some View {
        HStack {
            landmark.image
                .resizable()
                .frame(width: 50, height: 50)
            Text(landmark.name)
            Spacer()
        }
    }
```

很简单，landmark.image得到了一个Image数据类型，后面通过resizeable()属性设置图片尺寸适配，frame设置了宽高。

效果如图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583141204119-e8bcdbee-8521-4cbd-a319-9c8511760f75.png#align=left&display=inline&height=584&name=image.png&originHeight=1168&originWidth=784&size=51084&status=done&style=none&width=392)

#### 预览
`previewLayout`
通过`.previewLayout`可以设置右边预览的显示区域：
```swift
static var previews: some View {
     LandmarkRow(landmark: landmarkData[1])
        .previewLayout(.fixed(width: 300, height: 70))
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583141426919-64ca09a1-bbef-48af-8948-1540540d2507.png#align=left&display=inline&height=131&name=image.png&originHeight=262&originWidth=684&size=34199&status=done&style=none&width=342)

可以通过Group来显示多个预览：

```swift
Group {
    LandmarkRow(landmark: landmarkData[0])
          .previewLayout(.fixed(width: 300, height: 70))
    LandmarkRow(landmark: landmarkData[1])
          .previewLayout(.fixed(width: 300, height: 70))
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583141570166-02499482-ab86-4941-8892-7c62075ccd32.png#align=left&display=inline&height=254&name=image.png&originHeight=508&originWidth=666&size=68492&status=done&style=none&width=333)

另一种写法：
```swift
Group {
    LandmarkRow(landmark: landmarkData[0])
    LandmarkRow(landmark: landmarkData[1])
}.previewLayout(.fixed(width: 300, height: 70))
```

效果一样。

### 创建List
新建一个`LandmarkList.swift`
UI代码如下：
```swift
struct LandmarkList: View {
    var body: some View {
        List {
            LandmarkRow(landmark: landmarkData[0])
            LandmarkRow(landmark: landmarkData[1])
        }
    }
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583142241296-4bdc205c-6cf1-46c4-b835-80e095f6eca4.png#align=left&display=inline&height=188&name=image.png&originHeight=376&originWidth=730&size=54295&status=done&style=none&width=365)

### 使List内容跟随数据源生成

结构图：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583142297118-68409893-fb61-4c08-a03f-52a24fabe3d6.png#align=left&display=inline&height=371&name=image.png&originHeight=742&originWidth=697&size=62394&status=done&style=none&width=348.5)

代码：
```swift
List(landmarkData, id: \.id) { landmark in
	LandmarkRow(landmark: landmark)
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583142396053-2f74f9b4-8237-4e77-adcd-8f176fbb684c.png#align=left&display=inline&height=504&name=image.png&originHeight=1008&originWidth=506&size=127599&status=done&style=none&width=253)

#### 简化代码
由于`Landmark`类型已经具有协议`id`要求的属性`Identifiable`，因此可以省去`List`要求的对`id`的指定。
先对`Landmark`添加`Identifiable`：
```swift
struct Landmark: Hashable, Codable, Identifiable
```

修改后的LandmarkList：
```swift
List(landmarkData){ landmark in
	LandmarkRow(landmark: landmark)
}
```

### 跳转
和安卓不同，Swift的跳转似乎很方便，只需要在List外圈包围一个NavigationView即可。
代码如下：
```swift
NavigationView {
	List(landmarkData) { landmark in
		LandmarkRow(landmark: landmark)
	}.navigationBarTitle(Text("Landmarks"))
}
```

通过navigationBarTitle可以设置当前Title。

通过NavigationLink可以定向跳转的界面，代码如下：
```swift
NavigationView {
    NavigationLink(destination: LandmarkDetail()) {
		LandmarkRow(landmark: landmark)
	}
}.navigationBarTitle(Text("Landmarks"))
```

#### 数据传递
将跳转代码内填入需要传递的参数。
```swift
 NavigationLink(destination: LandmarkDetail(landmark: landmark))
```

LandmarkDetail.swift页面修改：
```swift
struct LandmarkDetail: View {
    var landmark: Landmark
    
    var body: some View {
        VStack {
            MapView(coordinate: landmark.locationCoordinate)
                .edgesIgnoringSafeArea(.top)
                .frame(height: 300)

            CircleImage(image: landmark.image)
                .offset(x: 0, y: -130)
                .padding(.bottom, -130)

            VStack(alignment: .leading) {
                CircleImage(image: landmark.image)
                    .font(.title)
                HStack(alignment: .top) {
                    Text(landmark.park)
                        .font(.subheadline)
                    Spacer()
                    Text(landmark.state)
                        .font(.subheadline)
                }
            }
            .padding()

            Spacer()
        }
        .navigationBarTitle(Text(landmark.name), displayMode: .inline)
    }
}

struct LandmarkDetail_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkDetail(landmark: landmarkData[0])
    }
}
```

效果：
![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583144989310-b48f0ee9-5615-4b3c-9f8d-0953ed6efc6d.png#align=left&display=inline&height=571&name=image.png&originHeight=1522&originWidth=768&size=368993&status=done&style=none&width=288)![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583145094053-a321e916-9d29-4bb5-b07d-15754dd6ba9a.png#align=left&display=inline&height=574&name=image.png&originHeight=1522&originWidth=768&size=682016&status=done&style=none&width=290)
