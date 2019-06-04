# Ubuntu 16.04 使用 Sogou pinyin 输入法遇到的问题

可能是使用的时间太长，导致在输入某些词时异常崩溃，重启sogou输入法是一种解决方法，但是更好的解决方法可能是：

1. 删除配置文件

ubuntu下搜狗的配置文件在 ~/.config下的3个文件夹里：

SogouPY、SogouPY.users、sogou-qimpanel

``` sh
cd .config
rm -r SogouPY*
rm -r sogou-qimpanel
```

2. 重启 fcitx

``` sh
//结束fcitx进程
killall fcitx
//重启fcitx
fcitx
```

https://blog.csdn.net/selous/article/details/70317130