# Caffe 问题总结

## 训练和测试时 BatchNorm 层的参数

> 有时候训练网络会不收敛，可能时 BN 层参数设置不对

- 训练时，batch_norm_param 中的 use_global_stats 设置为 false
- 测试时，batch_norm_param 中的 use_global_stats 设置为 true

## 找不到内置层

- 问题日志

``` text
F0411 09:37:03.688216 30007 layer_factory.hpp:81] Check failed: registry.count(type) == 1 (0 vs. 1) Unknown layer type: MemoryData (known types: FaceData)
```

- 原因及解决方法

主要是由于 caffe 作为静态库链接在可执行程序中时，由于链接方式的不一样，有部分程序没有链接进去，在这里体现为部分网络层的注册代码没有链接进去。所以在链接 caffe 静态库时，应使用 `-Wl， --whole-archive` 参数进行链接。

> 参考：https://stackoverflow.com/questions/30325108/caffe-layer-creation-failure#

## 卷积层中的 CuDNN 问题

- 问题日志

``` text
Check failed: status == CUDNN_STATUS_SUCCESS (3 vs. 0) CUDNN_STATUS_BAD_PARAM
```

- 原因及解决方法

原因不清楚，解决方法是在卷积层convolution_param中添加 `engine: CAFFE`

> 参考：https://blog.csdn.net/renhanchi/article/details/78500665

## 限制某些网络层不反传

layer 中有一个参数，propagate_down，设置为 0 时，该层以后的都不反传

例如：

``` text
layer {
  name: "loss"
  type: "EuclideanLoss"
  bottom: "Fc_1"
  bottom: "shufflenet_output_512"
  top: "loss"
  propagate_down: 0
  propagate_down: 1
}
```

## 不打印网络日志

代码中加入

``` cpp
// 在程序最开始运行地方，加入
::google::InitGoogleLogging("");  // 初始化
FLAGS_stderrthreshold = ::google::ERROR;  // 只打印ERROR级别信息
// 在程序结束的时候加入代码
google::ShutdownGoogleLogging();
```

## Caffe 编译报错

``` text
/home/zhi/anaconda3/lib/libpng16.so.16: undefined reference to `inflateValidate@ZLIB_1.2.9'
...
```

解决：

在 Makefile 中加入

``` text
LINKFLAGS := -Wl,-rpath,$(HOME)/anaconda3/lib
```

## 测试阶段 DuDNN 报错

训练时没报错，但测试阶段报错（测试使用的是 Caffe Python 接口）

``` text
F0906 23:21:33.909919 32434 cudnn_conv_layer.cu:28] Check failed: status == CUDNN_STATUS_SUCCESS (8 vs. 0)  CUDNN_STATUS_EXECUTION_FAILED
*** Check failure stack trace: ***
Aborted (core dumped)
```

排除网上说的显卡设备计算能力弱的原因（GTX 1080Ti），解决方法类似于如下：

``` shell
export CUDA_VISIBLE_DEVICES=2
```