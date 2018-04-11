# Caffe 问题总结

- 问题日志

``` text
F0411 09:37:03.688216 30007 layer_factory.hpp:81] Check failed: registry.count(type) == 1 (0 vs. 1) Unknown layer type: MemoryData (known types: FaceData)
```

- 原因及解决方法

主要是由于 caffe 作为静态库链接在可执行程序中时，由于链接方式的不一样，有部分程序没有链接进去，在这里体现为部分网络层的注册代码没有链接进去。所以在链接 caffe 静态库时，应使用 `-Wl， --whole-archive` 参数进行链接。

> 参考：https://stackoverflow.com/questions/30325108/caffe-layer-creation-failure#