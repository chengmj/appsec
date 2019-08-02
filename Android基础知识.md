# Android基础知识
学习链接：https://github.com/OWASP/owasp-mstg/blob/master/Document/0x05a-Platform-Overview.md

## Android体系结构

### ![timg](android-images/timg.jpg)

### Linux内核层

Android系统是基于Linux内核的，这一层为Android设备的各种硬件提供了底层的驱动（如显示，音频，照相机，蓝牙，WI-FI，电源管理等等）。

### Android运行时环境

应用程序代码（java）--->编译成class文件--->Dx将class文件编译成dex格式文件--->dex在DVM（Dalvik虚拟机）中运行。

这与JVM不一样。

**Dalvik**虚拟机：Android程序不同于J2ME程序（是java的一种运行环境），每个Android应用都运行在自己的进程上，享有Dalvik虚拟机为它分配的专有实例，并在该实例中执行。Dalvik虚拟机是一种基于寄存器的Java虚拟机，而不是传统的基于栈的虚拟机，并进行了内存资源使用的优化以及支持多个虚拟机的特点。设计成在一个设备可以高效地运行多个虚拟机。Dalvik虚拟机可执行文件格式是（.dex），dex格式是专为Dalvik设计的一种压缩格式，适合内存和处理器速度有限的系统。 大多数虚拟机包括JVM都是基于栈的，而Dalvik虚拟机则是基于寄存器的，所有的类都经由JAVA编译器编译，然后通过SDK中 的 "dx" 工具转化成.dex格式由虚拟机执行。Java编译器将Java源文件转为class文件，class文件又被内置的dx（具有转化为dex格式文件，该格式文件针对最小内存使用做了优化，这种文件在Dalvik虚拟机上注册并运行）。在一些底层功能方面，例如线程和低内存管理等.Dalvik虚拟机是依赖Linux内核的。

### 应用程序框架

这一层主要提供了构建应用程序时可能用到的各种API。

该框架包含：活动管理器、窗口管理器、内容提供者、视图系统、包管理器、电话管理器、资源管理器、位置管理器、通知管理器、XMPP服务。



- **活动管理器:** 管理各个应用程序的生命周期以及通常的导航回退功能。作用：负责一新ActivityThread进程创建，Activity生命周期的维护。其自身也存在一个框架，本文就不再讨论，有兴趣的可以看一看。



-  **窗口管理器:** 管理所有的窗口程序，在安卓应用框架中窗口主要分为两种:

（一）是应用窗口（一个activity有一个主窗口，弹出的对话框也有一个窗口，Menu菜单也是一个窗口。在同一个activity中，主窗口、对话框、Menu窗口之间通过该activity关联起来）。

（二）是公共界面的窗口（系统级别的窗口如：最近运行对话框、关机对话框、状态栏下拉栏、锁屏界面等）窗口管理系统是基于C/S模式的。整个窗口系统分为服务端和客户端两大部分，客户端负责请求创建窗口和使用窗口，服务端完成窗口的维护，窗口显示等。



 **内容提供器**：使得不同应用程序之间存取或者分享数据。就是可以配置自己的Content Provider去存取其他的应用程序或者通过其他应用程序暴露的Content Provider去存取它们的数据，总的来说就是提供了一个数据共享机制。



 **视图系统:** 构建应用程序的基本组就是文本框、按钮等。



 **通告管理器:** 使得应用程序可以在状态栏显示自定义的提示信息，通过NotificationManager 、 Notification这两个类可以完成在状态栏显示提示的信息。



**包管理器:** 安卓系统内的程序管理，Package Manger是一个实际上管理应用程序安装、卸载和升级的API。当我们安装APK文件时，Package Manager会解析APK包文件和显示确认信息。

​     

**电话管理器:** 管理所有的移动设备 用于管理手机通话状态、获取电话信息（设备、sim卡、网络信息），监听电话状态以及调用电话拨号器拨打电话。



**资源管理器:** 提供应用程序使用的各种非代码资源。提供应用程序使用的各种非代码资源，如本地化字符串、图片、布局文件、颜色文件等

​        

**位置管理器:** 提供位置服务，LocationManager系统服务是位置服务的核心组件，它提供了一系列方法来处理与位置相关的问题，包括查询上一个已知位置、注册和注销来自某个。LocationProvider的周期性的位置更新、注册和注销接近某个坐标时对一个已定义的Intent的触发等。总的来说就是提供有关位置的操作。

​     

**XMPP服务:例如**提供Google Talk 服务，XMPP（Extensible Messageing and Presence Protocol：可扩展消息与存在协议）：是一种即时消息协议用于信息的传输。是一种基于XML的开放式实时通信协议，XMPP是基于服务器的也是分散式的

### 应用程序组件
Android应用程序由多个组件构成，其中主要的4种组件有：Activity、Broadcast Receiver、Content Provider和Service。
#### Activity
Android应用程序的可视用户接口，即用户看到的与之进行可视交互的屏幕显示界面。
大多数app都会包含多个Activity，每一个Activity对应一个用户能够看到的或与之交互的屏幕显示界面，在这些Activity之间，用户可以随意的切换。
用户能够以任何顺序（除少量例外）启动app中不同的Activity。还可以使用Intent启动其他app的Activity。
app启动时，会有一个main Activity被启动，作为UI显示在屏幕上。
开发者使用setContentView函数创建UI组件，也就是显示屏幕显示界面的布局。

**所有的Activity都需要在Manifest文件内声明。** 否则不能在系统中注册，未声明的Activity也就无法被启动。
Manifest文件中使用`<activity>`标签声明Activity类。

**Android应用程序可以启动其他应用程序的Activity**，因此需要对这种特定的能力进行限制。
在Manifest文件中，其他应用程序使用`use-permission`权限标签，请求对应权限功能的访问。Activity可以使用`<activity>`标签的`ndroid:permission`属性设置Activity的权限，限制其他应用对该Activity的启动。
当其他app调用`Context.startActivity`()或`Activity.startActivityForResult()`启动该Activity时，系统会查看其权限，若调用者不具有该权限，启动Activity的请求会被拒绝。

#### Intent
Intent用于激活其他应用程序组件（如Activity、Service和Broadcast Receiver）的消息体。描述了需要执行的动作或操作。
借助Intent，Android提供了一种在运行之后的应用程序组件之间的绑定机制，这种机制既可以在同一个app的组件之间运行，也可以在不同的app组件之间运行。
例如：
阅读一篇文章，想将文章分享给别人，就会发生在A应用调用B应用Activity的活动。
  
### 系统中的权限查看
在设备上，没有用于应用程序的Manifest XML文件，Manifest XML文件只用在开发人员编写创建apk文件时，对于已经安装在系统上的应用程序来说，如果想要查看程序的权限，只能通过查阅设备系统上/data/system目录下的packages.xml文件。

需要系统执行权限验证的情景有：
- 应用程序执行时。
- 应用程序执行没有授权的功能时。
- 应用程序启动没有授权的Activity时。
- 应用程序发送或接收广播Intent时。
- 访问或更新Content Provider时。
- 应用程序创建服务时。

### android:exported
Whether or not components of other applications can invoke the service or interact with it — "true" if they can, and "false" if not. When the value is "false", only components of the same application or applications with the same user ID can start the service or bind to it.    
The default value depends on whether the service contains intent filters. The absence of any filters means that it can be invoked only by specifying its exact class name. This implies that the service is intended only for application-internal use (since others would not know the class name). So in this case, the default value is "false". On the other hand, the presence of at least one filter implies that the service is intended for external use, so the default value is "true".    

This attribute is not the only way to limit the exposure of a service to other applications. You can also use a permission to limit the external entities that can interact with the service (see the permission attribute).    
参考链接：https://developer.android.com/guide/topics/manifest/service-element#exported

