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

#### 问题

```
nvidia-docker | 2018/06/08 17:20:41 Error: Could not load UVM kernel module. Is nvidia-modprobe installed?
```

solve ->

```
sudo apt install nvidia-modprobe
# 重装 nvidia-docker
...
```

### 更新：安装 NVIDIA-Docker 2.0

#### 卸载 nvidia-docker 1.0

``` shell
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f

sudo apt-get purge nvidia-docker
```

#### 安装 nvidia-docker 2.0

``` shell
# 配置仓库
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
# 安装 nvidia-docker2
sudo apt-get install nvidia-docker2
sudo pkill -SIGHUP dockerd
```

#### 参考

1. [Docker - 基于NVIDIA-Docker的Caffe-GPU环境搭建](http://blog.csdn.net/zziahgf/article/details/72578273)
2. [官方安装指南](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))