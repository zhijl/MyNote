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

#### 参考

1. [error while loading libstdc++.so.6](https://jingyan.baidu.com/article/cdddd41c820b5d53cb00e100.html)