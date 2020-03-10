# MacOS基础控件

# NSAlert

新建弹窗函数：
```swift
func dialogOKCancel(question: String, text: String) -> Bool {
	let alert = NSAlert()
	alert.messageText = question
	alert.informativeText = text
	alert.alertStyle = .warning
	alert.addButton(withTitle:"OK")
	alert.addButton(withTitle:"Cancel")
	return alert.runModal() == .alertFirstButtonReturn
}
```
该函数会返回一个布尔值，第一个button为true，其余的为false

其他参数参考[https://developer.apple.com/documentation/appkit/nsalert](https://developer.apple.com/documentation/appkit/nsalert)

调用：
```swift
let answer = dialogOKCancel(question:"Ok?", text:"Choose your answer.")

if answer {
	print("TRUE")
}else{
	print("FALSE")
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583401151216-5584cd6e-7820-4e47-9f42-fadc50e68184.png#align=left&display=inline&height=151&name=image.png&originHeight=302&originWidth=840&size=37019&status=done&style=none&width=420)

![image.png](https://cdn.nlark.com/yuque/0/2020/png/736116/1583401179565-ed4f9469-51bf-4a7c-862e-ec0cedbbebfe.png#align=left&display=inline&height=59&name=image.png&originHeight=118&originWidth=380&size=6534&status=done&style=none&width=190)






