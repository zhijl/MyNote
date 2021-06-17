# CUDA 安装和 NVIDIA 驱动安装

## 硬件及系统

- Ubuntu 16.04 64位
- GTX 1080

## NVIDIA 驱动及 CUDA / CuDNN 安装

### NVIDIA 驱动安装

```
sudo add-apt-repository ppa:xorg-edgers/ppa
sudo apt-get update
sudo apt-get install nvidia-375 #此处对应自己安装的的驱动版本
```

安装新版驱动前需卸载当前的驱动，如果是如源安装的，则通过 apt 的方式卸载，如果是通过 `run` 方式，最好的方式是，通过 NVIDIA 提供的卸载脚本进行卸载

卸载命令位置 `/usr/bin/nvidia-uninstall`，以下命令即可卸载

```
sudo /usr/bin/nvidia-uninstall
```

不找这个命令的位置，也可以

```
sudo apt install autoremove --purge nvidia*
```

plus:

```
sudo apt-cache search nvidia  #查找最新版本的驱动
```

```
sudo add-apt-repository ppa:graphics-drivers
sudo apt-get update
sudo apt-get install nvidia-430 #此处对应自己安装的的驱动版本
```

通过官方下载得到的 run 文件进行安装，需切换到纯命令行模式(CTRL+ALT+F1)，并执行如下命令：

```
sudo service lightdm stop 或者 sudo stop lightdm
sudo init 3
```

执行成功后，再运行 run 文件，按提示步骤安装即可

```
./NVIDIA-Linux-x86_64-450.57.run
```

注：Ubuntu 16.04 的 apt 源中最新的驱动只支持到 430，如果需要安装最新的驱动只能通过官方下载最新的驱动进行安装，例如使用上述 run 文件的安装方案。无论哪种安装方式都需要提前卸载系统中当前的驱动。

**特殊问题**

```
root@ubuntu-server# ./NVIDIA-Linux-x86_64-450.57.run 
Verifying archive integrity... OK
Uncompressing NVIDIA Accelerated Graphics Driver for Linux-x86_64 450.57................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................
Received signal SIGBUS; aborting.
```

```
root@ubuntu-server# nvidia-installer -v
nvidia-installer:  version 418.87.00
  The NVIDIA Software Installer for Unix/Linux.

  This program is used to install, upgrade and uninstall The NVIDIA Accelerated Graphics Driver Set for
  Linux-x86_64.
```

在执行卸载时发生了 SIGBUS ，可通过 `strace nvidia-installer --uninstall -s` 定位程序运行到哪报的错

例如在更新 460 版本的驱动时：

```
root@ubuntu-server# ./nvidia-installer --no-opengl-files --no-nouveau-check --no-x-check --no-ncurses-color > temp.log 2>&1
```

查看 temp.log 中的日志，发现是安装脚本在对 `/usr/lib/x86_64-linux-gnu/libcuda.so.418.87.00.cp` 进行 open write 时出错，直接删除该文件后，再次安装一切正常

### CUDA 安装

下载对应版本的 cuda_8.0.61_375.26_linux.run

```
sudo ./cuda_8.0.61_375.26_linux.run
```

在 ~/.bashrc 或 /etc/profile 中添加

```
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64
```

### cuDNN 安装

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

## 最新：CUDA 9 安装

代理情况下官网下载 deb 安装包

1. `sudo dpkg -i cuda-repo-ubuntu1604-9-2-local_9.2.148-1_amd64.deb`
2. `sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub`
3. `sudo apt-get update`
4. `sudo apt-get install cuda`