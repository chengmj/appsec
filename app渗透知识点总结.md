## Android渗透测试知识点总结
### 日志查看
- adb shell ps | grep appname    *查找app进程号*
- adb shell logcat | grep app_pid    *按进程号查看logcat日志*

### 不安全的数据存储
不安全的数据存储主要有三种方式：    
- 将敏感数据保存到配置文件中。
- 将敏感数据保存在本地的sqlite3数据库中。
- 将敏感数据保存在临时文件或者sd卡中。

对应的路径分别为：    
- **/data/data/apppackagename/shared_prefs**，如`/data/data/jakhar.aseem.diva/shared_prefs/jakhar.aseem.diva_preferences.xml`，对应的代码类有`SharedPreferences`。
- **/data/data/apppackagename/databases**，如`/data/data/jakhar.aseem.diva/databases/ids2`，对应的代码有创建数据库方法`openOrCreateDatabase`，执行SQL语句方法`execSQL`等。
- 临时文件如：`/data/data/jakhar.aseem.diva/uinfo-1681723197tmp`，相关代码如：`new File`, `File.createTempFile`, `FileWriter`等。
- sd卡路径一般为：`/mnt/sdcard`，注意查看文件时要用`ls -al`命令，以便查看隐藏文件。相关代码如：`Environment.getExternalStorageDirectory()`, `FileWriter`等。

### 访问控制
- 查看AndroidManifest.xml文件，查找有无`<intent-filter>`，如果有，当intent filter被activity等组件使用，这个activity可能会被外部任何应用程序所调用。如：`adb shell am start -a jakhar.aseem.diva.action.VIEW_CREDS`
- 上一步如果不成功，可以查看源代码看做了什么检查过滤操作，看是否能够绕过。如手动修改某参数值，`adb shell am start -a jakhar.aseem.diva.action.VIEW_CREDS2 --ez check_pn false`，查找参数主要依赖于反编译后的strings.xml和public.xml文件。
- 查看AndroidManifest.xml文件，查找有无` <provider`，ContentProvider的作用是为不同的应用之间数据共享，提供统一的接口。`android:enabled`表示是否能由系统初始化，`android:exported`表示是否能被其他应用使用，`android:authorities`标识这个ContentProvider，调用者可以根据这个标识来找到它，如果都为`true`，就可以用`content://`来访问，在程序中搜索`content://`关键字来查找uri，然后执行`content`命令，命令为：`adb shell content query –-uri content://jakhar.aseem.diva.provider.notesprovider/notes`

### 输入验证
- SQL注入
- file协议
- 缓冲区溢出

### 硬编码
- 查看源码，可能在class文件中，也可能在so文件中。