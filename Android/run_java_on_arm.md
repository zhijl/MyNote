# 在 Android 系统终端运行 Java 程序

> 主要在测试交叉编译的动态库是否存在依赖缺失时，会用Java写个小程序先测试一下

## 将 java 程序编译并打成 jar

这里使用 dx 进一步将 .class 文件打成 jar 包，dx 位于 Android SDK 的 build-tools 中

``` shell
javac runme.java
dx --dex --output=runme.jar runme.class package/*.class
```

## 将 jar 包 push 到 Android 平台，并运行

使用 dalvikvm 执行 jar 中的指定类

``` shell
adb push runme.jar /data
adb shell dalvikvm -cp /data/runme.jar runme
...
```
