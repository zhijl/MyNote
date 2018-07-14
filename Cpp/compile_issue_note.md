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

#### 交叉编译中遇到的 PIE 问题

问题日志

``` text
error: only position independent executables <PIE> are supported.
```

解决：

编译和链接时加上参数 `-pie -fPIE`

#### undefined reference to `__dso_handle'

在头文件中加入extern  `"C"{ void * __dso_handle = 0 ;}`

#### 参考

1. [error while loading libstdc++.so.6](https://jingyan.baidu.com/article/cdddd41c820b5d53cb00e100.html)