# Keras 安装

## Windows 平台

### Tenforlow 安装

注意，目前 Windows 平台的 Tensorflow 仅支持 Python3 版本

首先，打开Anaconda Prompt，通过如下命令进行安装

``` shell
# 切换到python3
activate python3.6
# GPU 版本
conda install tensorflow-gpu
# CPU 版本
conda install tensorflow
```

> 这里可能会提示先执行 `conda update -n base conda`，对 conda 进行更新

安装完 tensorflow 后，再执行

``` shell
pip list
```

可查看当前环境下安装的所有 Python 第三方包及对应版本

### Keras 安装

继续上面的 Anaconda Prompt 环境，执行如下语句

``` shell
conda install keras
```

## 参考

[1] [在不同版本python下安装tensorflow](http://blog.csdn.net/silence2015/article/details/73777071)
[2] [TensorFlow中文社区](http://www.tensorfly.cn/tfdoc/get_started/os_setup.html)