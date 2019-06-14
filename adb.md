
### adb连接模拟器    
adb connect 127.0.0.1    
这句命令默认会连5555端口，谷歌官方模拟器就是用这个端口，但是这些国产模拟器用的端口却不一样。    

windows下的    

夜神模拟器   adb connect 127.0.0.1:62001

逍遥模拟器 adb connect 127.0.0.1:21503

### adb用法
参考链接：https://github.com/mzlogin/awesome-adb

`adb devices`     -- 查看连接设备
```
List of devices attached
cf264b8f    device
emulator-5554   device
10.129.164.6:5555   device
```
