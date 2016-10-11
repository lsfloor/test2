# test2在编译项目之前，我们先确认一下编译环境。
请确保，Quick-Cocos2dx-Community、JDK、ADT、NDK 和 Ant 都已正确安装，这 5 个工具均需要配置"系统环境变量"。

具体的环境配置可以查看[此链接](http://www.tairan.com/archives/10567/)


作者的 Quick-Cocos2dx-Community 安装在D:\cocos\quick-3.3，并使用 player3 新建了一个 test 项目位于D:\cocos\workspace\hello。

>注：创建工程不勾选"Copy Source Files"，也就是不包含引擎代码。

现在就以 hello 项目为例说明如何编译 Android apk 包。

##步骤：

###第一步：切换到 hello 的 Android 工程路径。

这些操作都是在cmd命令行中进行操作

	C:\Users\Administrator>d:

	D:\>cd D:\cocos\workspace\hello\frameworks\runtime-src\proj.android


###第二步：清理编译临时文件。

	D:\cocos\workspace\hello\frameworks\runtime-src\proj.android>clean.bat

不一定每次都需要清理，但是建议编译 release 版本一定要先清理后打包。

编译 Quick-Cocos2dx-Community 引擎的 C++ 核心。

###第三步：
	D:\cocos\workspace\hello\frameworks\runtime-src\proj.android>build_native.bat

这个过程调用 NDK 进行编译，并生成 libcocos2dlua.so 文件。如果一切顺利，应该看到下面的 log 信息：

	[armeabi] SharedLibrary  : libcocos2dlua.so
	[armeabi] Install        : libcocos2dlua.so => libs/armeabi/libcocos2dlua.so
	make.exe: Leaving directory `D:/cocos/workspace/hello/frameworks/runtime-src/proj.android

###第四步：更新 test 的 Android 项目配置信息。

	D:\cocos\workspace\hello\frameworks\runtime-src\proj.android>android update project -p . -t 1

这步只需做一次即可。

###第五步：更新 Quick-Cocos2dx-Community 引擎工程的 Android 项目配置信息。

	D:\>cd D:\cocos\quick-3.3\cocos\platform\android\java

切换到cocos引擎安装目录下的\cocos\platform\android\java

	D:\cocos\quick-3.3\cocos\platform\android\java>android update project -p . -t 1

这步只需做一次即可。

###第六步：修改 项目的proj.android 目录下 project.properties 文件中关于 Quick-Cocos2dx-Community 引擎工程的引用路径。

android.library.reference.1=../../../../../../quick-3.3/cocos/platform/android/java

>注意以下事项：

>每次执行第 4 步骤后，都需要重新配置 project.properties。
>android.library.reference只认相对路径，所以 Quick-Cocos2dx-Community 和 test 一定要都在同一盘符下。

###第七步：切换回 test 工程下的 proj.android 目录，运行 ant 打包命令。

	D:\>cd D:\cocos\workspace\hello\frameworks\runtime-src\proj.android
	D:\cocos\workspace\hello\frameworks\runtime-src\proj.android>ant debug

注：如果遇到类似resolve to a path with no project.properties的错误提示信息，请仔细检查 project.properties 中的引擎相对路径是否正确配置。

成功编译将看到如下 log 信息：

	-post-build:

	debug:

	BUILD SUCCESSFUL
	Total time: 19 seconds

apk 文件位于proj.android/bin/hello-debug.apk。

ant 常用命令说明：

ant debug用于生成自签名的测试 apk 包，可直接在 Android 手机上安装运行。
ant release生成发布版本的 apk 包。默认配置生成的 hello-release-unsigned.apk 包是没有签名的，不能直接安装，还需要用签名工具进行签名。
ant clean清理 java 编译环境。
>注：clean.bat是清理 NDK 编译环境。
