## gcc 混合链接动态库和静态库

当对动态库与静态库混合连接的时候，使用-static会导致所有的库都使用静态连接的方式。这时需要作用`-Wl`的方式：

``` shell
gcc test.cpp -L. -Wl,-Bstatic -ltestlib  -Wl,-Bdynamic -ltestlib
```

注意`-Wl`会将选项参数转传给ld链接器

### 参考

1. [gcc 混合连接动态库和静态库](http://blog.csdn.net/sunlion81/article/details/9963963)