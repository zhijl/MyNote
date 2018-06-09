# CUDA 安装

## 硬件及系统

- Ubuntu 16.04 64位
- GTX 1080

## Nvidia 驱动及 CUDA / CuDNN 安装

Nvidia 驱动安装

```
sudo add-apt-repository ppa:xorg-edgers/ppa
sudo apt-get update
sudo apt-get install nvidia-375 #此处对应自己安装的的驱动版本
```

CUDA 安装

下载对应版本的 cuda_8.0.61_375.26_linux.run

```
sudo ./cuda_8.0.61_375.26_linux.run
```

在 ~/.bashrc 或 /etc/profile 中添加

```
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64
```

CuDNN 安装

下载对应版本的 cudnn-8.0-linux-x64-v7.1.tgz

```
sudo cp lib64/* /usr/local/cuda/lib64/
sudo cp include/cudnn.h /usr/local/cuda/include/
```

## Nvidia 驱动及 CUDA 卸载

卸载 Nvidia 驱动

```
sudo apt-get remove --purge nvidia-*
```

卸载 CUDA

``` shell
sudo /usr/local/cuda-8.0/bin/uninstall_cuda-8.0.pl
```

## 问题

```
Failed to initialize NVML: Driver/library version mismatch
```

solve ->

```
一般出现在重装 Nvidia 驱动时，解决方法是先尝试重启系统
```
