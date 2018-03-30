## NVIDIA-Docker 安装

#### 环境要求

``` text
GNU/Linux x86_64 with kernel version > 3.10 
Docker >= 1.9 (official docker-engine, docker-ce or docker-ee only) 
NVIDIA GPU with Architecture > Fermi (2.1) 
NVIDIA drivers >= 340.29 with binary nvidia-modprobe (驱动版本与CUDA计算能力相关)

CUDA 与 NVIDIA driver需预先安装好
```

#### NVIDIA-Docker 安装

``` shell
# Install nvidia-docker and nvidia-docker-plugin

wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb

# Test nvidia-smi

sudo nvidia-docker run --rm nvidia/cuda nvidia-smi
```

#### 参考

1. [Docker - 基于NVIDIA-Docker的Caffe-GPU环境搭建](http://blog.csdn.net/zziahgf/article/details/72578273)