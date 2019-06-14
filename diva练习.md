

D:\appsec\platform-tools_r29.0.1-windows\platform-tools>`adb shell ps| findstr di
va`  
u0_a4     10273 77    1541480 43112 ffffffff b7558ce5 S jakhar.aseem.diva  

D:\appsec\platform-tools_r29.0.1-windows\platform-tools>`adb shell logcat | finds
tr 10273`  
E/libprocessgroup(10273): failed to make and chown /acct/uid_10004: Read-only fi
le system  
I/ActivityManager(  420): Start proc 10273:jakhar.aseem.diva/u0a4 for activity j
akhar.aseem.diva/.MainActivity  
D/OpenGLRenderer(10273): endAllStagingAnimators on 0xb3f53a80 (RippleDrawable) w
ith handle 0xadc297c0  
E/diva-log(10273): Error while processing transaction with credit card: **12312312
31234**   
V/RenderScript(10273): 0xa14ca400 Launching thread(s), CPUs 2  

