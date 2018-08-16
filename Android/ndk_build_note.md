# NDK-build 常用命令参数

- ndk-build NDK_LOG=1

用于配置 LOG 级别，打印 ndk 编译时的详细输出信息

- ndk-build NDK_PROJECT_PATH=.

指定 ndk 编译的代码路径为当前目录，如果不配置，则必须把工程代码放到 Android 工程的 jni 目录下

- ndk-build APP_BUILD_SCRIPT=./Android.mk

指定 NDK 编译使用的 Android.mk 文件

- ndk-build NDK_APP_APPLICATION_MK=./Application.mk

指定 NDK 编译使用的 application.mk 文件

- ndk-build clean

清除所有编译出来的临时文件和目标文件

- ndk-build -B

强制重新编译已经编译完成的代码

- ndk-build NDK_DEBUG=1

执行 debug build

- ndk-build NDK_DEBUG=0

执行 release build

- ndk-build NDK_OUT=./mydir

指定编译生成的文件的存放位置

- ndk-build -C /opt/myTest/

到指定目录编译 native 代码

参考：

https://www.linuxidc.com/Linux/2014-12/110167.htm