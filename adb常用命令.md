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



### apktool
#### 解包：  
```
D:\tools>java -jar  apktool_2.3.4.jar  d -f apk D:\虚拟机导出\diva-beta.apk -o
:\虚拟机导出\diva-test
I: Using Apktool 2.3.4 on diva-beta.apk
I: Loading resource table...
I: Decoding AndroidManifest.xml with resources...
S: WARNING: Could not write to (C:\Users\zte\AppData\Local\apktool\framework),
sing C:\Users\zte\AppData\Local\Temp\ instead...
S: Please be aware this is a volatile directory and frameworks could go missin
 please utilize --frame-path if the default storage directory is unavailable
I: Loading resource table from file: C:\Users\zte\AppData\Local\Temp\1.apk
I: Regular manifest package...
I: Decoding file-resources...
I: Decoding values */* XMLs...
I: Baksmaling classes.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Copying original files...

D:\tools>
```
#### 打包：  
```
D:\tools>java -jar  apktool_2.3.4.jar  b -f  D:\虚拟机导出\diva-test -o D:\虚拟
机导出\diva-test\diva-new.apk
I: Using Apktool 2.3.4
I: Smaling smali folder into classes.dex...
I: Building resources...
S: WARNING: Could not write to (C:\Users\zte\AppData\Local\apktool\framework), u
sing C:\Users\zte\AppData\Local\Temp\ instead...
S: Please be aware this is a volatile directory and frameworks could go missing,
 please utilize --frame-path if the default storage directory is unavailable
W: fakeLogOpen(/dev/log_crash) failed
W: fakeLogOpen(/dev/log_stats) failed
W: fakeLogOpen(/dev/log_stats) failed
I: Copying libs... (/lib)
I: Building apk file...
I: Copying unknown files/dir...
I: Built apk...

D:\tools>
```
### apk签名
keytool -genkey -alias ztetest.keystore -keyalg RSA -validity 2000 -keystore newdiva.keystore
#### 签名文件的生成：
```
D:\appsec\platform-tools_r29.0.1-windows\platform-tools>D:\tools\jdk-11.0.2_wind
ows-x64_bin\jdk-11.0.2\bin\keytool.exe -genkey -alias ztetest.keystore -keyalg R
SA -validity 2000 -keystore newdiva.keystore
输入密钥库口令:
再次输入新口令:
您的名字与姓氏是什么?
  [Unknown]:  cmj
您的组织单位名称是什么?
  [Unknown]:  zte
您的组织名称是什么?
  [Unknown]:  zte
您所在的城市或区域名称是什么?
  [Unknown]:  nj
您所在的省/市/自治区名称是什么?
  [Unknown]:  js
该单位的双字母国家/地区代码是什么?
  [Unknown]:  cn
CN=cmj, OU=zte, O=zte, L=nj, ST=js, C=cn是否正确?
  [否]:  是
```
#### 签名：
使用jarsigner进行签名
jarsigner -verbose -keystore [您的私钥存放路径] -signedjar [签名后文件存放路径] [未签名的文件路径] [您的证书名称]
[您的证书名称] 即keytool生成签名文件时的别名参数，即-alias 后面的参数。   
```
D:\tools>D:\tools\jdk-11.0.2_windows-x64_bin\jdk-11.0.2\bin\jarsigner.exe -verbo
se -keystore D:\appsec\platform-tools_r29.0.1-windows\platform-tools\newdiva.key
store -signedjar D:\虚拟机导出\diva-test\diva-test-signed.apk D:\虚拟机导出\diva
-test\diva-new.apk ztetest.keystore
```


