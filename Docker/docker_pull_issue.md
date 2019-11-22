# Docker 使用过程中遇到的问题记录

## 拉取私有仓库镜像

### 问题

要拉取 192.168.1.55:5000/caffe/cuda:latest 这个镜像

但是报错：

``` text
Error response from daemon: Get https://192.168.1.55:5000/v2/: http: server gave HTTP response to HTTPS client
```

### 解决方法

- 修改 `/etc/docker/daemon.json` 文件
- 在文件中加入 `"insecure-registries":["193.168.1.117:5000"]`

即

``` text
{
  "insecure-registries":["193.168.1.117:5000"],
  "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

- 最后重启 docker 即可

``` sh
systemctl daemon-reload
systemctl restart docker
```

参考

https://blog.csdn.net/qq_29257691/article/details/100535361


## `...no space left on device`

### 问题

在拉取一个镜像的时候，提示

```
latest: Pulling from caffe/cuda
a02a4930cb5d: Pull complete 
56f7f0b3fd8b: Pull complete 
b71964dd39ad: Pull complete 
fc2b1c336759: Pull complete 
bc759b41450c: Pull complete 
30238acf9c23: Pull complete 
c2cb776f8e56: Extracting [==================================================>]  543.3MB/543.3MB
d494345d0946: Download complete 
00512d77c3f7: Download complete 
eb1dfbc99eb6: Download complete 
cd199ca5aab3: Download complete 
3922302683e0: Download complete 
a3b0120b26a5: Download complete 
4e3f7a40482c: Download complete 
failed to register layer: Error processing tar file(exit status 1): write /usr/local/cuda-9.1/targets/x86_64-linux/lib/libcusolver_static.a: no space left on device
```

应该是存放 docker 的磁盘分区空间不够导致的

### 分析

**查看 docker 根目录**

``` sh
docker info | grep -i "docker root dir"

...
 Docker Root Dir: /var/lib/docker
...
```

一般默认为 `/var/lib/docker`

**查看 docker 根目录所占磁盘空间大小**

``` sh
df -hl /var/lib/docker

Filesystem      Size  Used Avail Use% Mounted on
/dev/sda8        14G   12G  2.1G  85% /
```

可见所剩余的空间只有 2.1 GB 了

### 解决方法

我们可以把 docker 的根目录改到另一个地方

- 创建目标目录 `mkdir -p /etc/systemd/system/docker.service.d/`
- 创建配置文件，配置文件中的 `/docker-root` 为 docker 新的根目录

``` text
cat /etc/systemd/system/docker.service.d/devicemapper.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd  --graph=/docker-root
```

- 重启 docker

``` sh
systemctl daemon-reload
systemctl restart docker
```
