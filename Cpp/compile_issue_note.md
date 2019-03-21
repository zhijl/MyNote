## 编译中遇到的问题

#### 交叉编译 gcc 运行时缺少 libstdc++.so.6

``` text
arm-linux-gnueabihf-gcc: error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory
```

原因： 缺少了依赖文件lib32stdc++6

解决：

``` shell
sudo apt-get install lib32stdc++6
```

#### 交叉编译中遇到 `xxx.so: unexpected reloc type 0x03` 问题

主要是在编译库时，没有使用 `-fPIC`

通过如下指令查看该库

``` sh
readelf -r libtest.so|grep R_ARM_REL32
```

如果使用- fPIC，这个函数的重定向类型是 R_ARM_GLOB_DAT

引用：

https://www.cnblogs.com/sciapex/p/9662811.html

#### 交叉编译中遇到的 PIE 问题

问题日志

``` text
error: only position independent executables <PIE> are supported.
```

解决：

编译和链接时加上参数 `-pie -fPIE`

#### undefined reference to `__dso_handle'

在头文件中加入extern  `"C"{ void * __dso_handle = 0 ;}`

#### 交叉编译 Android 版本的库时 `__android_log_print`

``` text
undefined reference to '__android_log_print'
```

在 android.mk 文件中找到

include $(CLEAR_VARS)

在下面增加一行：

``` Makefile
LOCAL_LDLIBS := -lm -llog
```

#### 参考

1. [error while loading libstdc++.so.6](https://jingyan.baidu.com/article/cdddd41c820b5d53cb00e100.html)
2. [JNI编程-- undefined reference to `__android_log_print' 的解决办法](https://blog.csdn.net/forandever/article/details/50393499)