# 使用 PaddlePaddle 中遇到的问题

### module compiled against

> 执行 `paddle.init(use_gpu=with_gpu, trainer_count=8)` 时报了下列异常

``` text
RuntimeError: module compiled against API version 0xc but this version of numpy is 0xa
......
ImportError: numpy.core.multiarray failed to import
```

**可能原因：**

1. Numpy 版本过低，尝试重新安装 Numpy，但可能问题依旧
1. Cuda 版本与 paddlepaddle-gpu 编译时所用的版本不匹配，重新下载与当前环境相匹配的 paddlepaddle-gpu 版本

**总结：**

pip 安装的 gpu 版本深度学习框架的编译版本的 CUDA 和 CuDNN 一定要与系统版本当前配置的版本一致

### paddle.fluid.core.EnforceNotMet: enforce allocating <= available failed, 10238983668 > 9509207808

paddle.fluid.core.EnforceNotMet: enforce allocating <= available failed, 10238983668 > 9509207808

通过如下变量设定使用的显存占用的比例

```
export FLAGS_fraction_of_gpu_memory_to_use=0.1
```