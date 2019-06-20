
## 1. Insecure Logging

D:\appsec\platform-tools_r29.0.1-windows\platform-tools>**`adb shell ps| findstr diva`**  
u0_a4     10273 77    1541480 43112 ffffffff b7558ce5 S jakhar.aseem.diva  

D:\appsec\platform-tools_r29.0.1-windows\platform-tools>**`adb shell logcat | findstr 10273`**  
E/libprocessgroup(10273): failed to make and chown /acct/uid_10004: Read-only fi
le system  
I/ActivityManager(  420): Start proc 10273:jakhar.aseem.diva/u0a4 for activity j
akhar.aseem.diva/.MainActivity  
D/OpenGLRenderer(10273): endAllStagingAnimators on 0xb3f53a80 (RippleDrawable) w
ith handle 0xadc297c0  
E/diva-log(10273): Error while processing transaction with credit card: **12312312
31234**   
V/RenderScript(10273): 0xa14ca400 Launching thread(s), CPUs 2  

可以看到logcat打印出输入的信用卡信息。

### logcat知识点

Android Logcat使用起来可以方便的观察调试内容，基本上的使用方法(巧用Logcat调试程序)。

一、Log.v 的调试颜色为黑色的，任何消息都会输出，这里的v代表verbose啰嗦的意思，平时使用就是Log.v(“”,”“);

二、Log.d的输出颜色是蓝色的，仅输出debug调试的意思，但他会输出上层的信息，过滤起来可以通过DDMS的Logcat标签来选择

三、Log.i的输出为绿色，一般提示性的消息information，它不会输出Log.v和Log.d的信息，但会显示i、w和e的信息

四、Log.w的意思为橙色，可以看作为warning警告，一般需要我们注意优化Android代码，同时选择它后还会输出Log.e的信息。

五、Log.e为红色，可以想到error错误，这里仅显示红色的错误信息，这些错误就需要我们认真的分析，查看栈的信息了。

这些区别就是在DDMS的Logcat显示的颜色的区别

## 2. Hardcoding Issues    
通过查看HardcodeActivity.java文件，可以看到hckey与vendorsecretkey比较，将vendorsecretkey输入即可。
```

    public void access(View view) {
        EditText hckey = (EditText) findViewById(R.id.hcKey);

        if (hckey.getText().toString().equals("vendorsecretkey")) {
            Toast.makeText(this, "Access granted! See you on the other side :)", Toast.LENGTH_SHORT).show();
        }
        else {
            Toast.makeText(this, "Access denied! See you in hell :D", Toast.LENGTH_SHORT).show();
        }
    }
 ```

![硬编码](2019-06-17_215440.jpg)

## 4.Insecure Data Storage -Part2
数据库路径：/data/data/jakhar.aseem.diva/databases
```
root@SM-G930F:/data/data/jakhar.aseem.diva/databases # ls
divanotes.db
divanotes.db-journal
ids2
ids2-journal
root@SM-G930F:/data/data/jakhar.aseem.diva/databases #
root@SM-G930F:/data/data/jakhar.aseem.diva/databases # sqlite3 ids2
SQLite version 3.8.6.1 2015-05-21 17:24:32
Enter ".help" for usage hints.
sqlite> show tables;
Error: near "show": syntax error
sqlite> .tables
android_metadata  myuser
sqlite> select * from myuser;
test_name|test_password2
sqlite>
```
