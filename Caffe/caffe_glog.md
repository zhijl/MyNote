# Caffe 日志在终端显示的问题

> 有时候在部署深度学习应用时不希望 Caffe 的日志在终端输出（暴露网络结构），网上找了一些方法，但是就算不在终端输出，还是会导致日志存为文件。所有当前暂时找不到特别好的方案。本文主要介绍一些简单的方法尽量屏蔽 Caffe 日志在终端输出。

## 方法

```
// 在程序启动前调用
::google::InitGoogleLogging(char* str);

// 在程序结束时调用
google::ShutdownGoogleLogging();
```

> `::google::InitGoogleLogging(char* str);` 一个进程中只能调用一次，多次调用会抛异常，所以在初始化时可以全局变量判断是否已经调用过

## 参考

1. [如何设置和关闭caffe的日志系统(glog)](https://blog.csdn.net/XiaoHeiBlack/article/details/54969642)