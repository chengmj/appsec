
### adb连接模拟器    
adb connect 127.0.0.1    
这句命令默认会连5555端口，谷歌官方模拟器就是用这个端口，但是这些国产模拟器用的端口却不一样。    

windows下的    

夜神模拟器   adb connect 127.0.0.1:62001

逍遥模拟器 adb connect 127.0.0.1:21503

### adb用法
参考链接：https://github.com/mzlogin/awesome-adb    
基本语法：    
`adb [-d|-e|-s <serialNumber>] <command>`    
如果只有一个设备/模拟器连接时，可以省略掉 [-d|-e|-s <serialNumber>] 这一部分，直接使用 adb <command>。
  
`adb devices`     -- 查看连接设备，如：    
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
