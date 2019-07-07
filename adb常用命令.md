### adb连接模拟器    
adb connect 127.0.0.1    
这句命令默认会连5555端口，谷歌官方模拟器就是用这个端口，但是这些国产模拟器用的端口却不一样。    

windows下的    

夜神模拟器   adb connect 127.0.0.1:62001

逍遥模拟器 adb connect 127.0.0.1:21503

### 查看已安装的包

adb shell pm list packages

### 卸载应用
先列出手机上所有应用信息：

`adb shell dumpsys package > ./package.txt`
从中找出你要的APP，重点关注 Activity Resolver Table 下面的内容。另外，看你的应用APK包的名字，搜索关键词

找到后:
强制关闭应用：

`adb shell am force-stop xxxxxx`
启动应用：

`shell am start -n xxxxxx/xxxx`

示例：

![2019-07-06_112257](diva-images\2019-07-06_112257.jpg)

### 查看设备

adb devices

```
$ adb devices
List of devices attached
cf264b8f    device
emulator-5554   device
10.129.164.6:5555   device
```

输出里的 cf264b8f、emulator-5554 和 10.129.164.6:5555 即为 serialNumber。

比如这时想指定 cf264b8f 这个设备来运行 adb 命令获取屏幕分辨率：    

```
adb -s cf264b8f shell wm size
```

又如：    

```
adb -s 10.129.164.6:5555 install test.apk
```







