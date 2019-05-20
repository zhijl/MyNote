# 使用 Docker 的过程中遇到的问题

### docker load ... : no space left on device

当 load 一个镜像文件时，提示 `no space left no device` 的错误

具体现象如下：

``` text
docker load --input rknn-toolkit-1.0.0-docker.tar.gz
0de2edf7bff4: Loading layer  121.3MB/121.3MB
b2fd8b4c3da7: Loading layer  15.87kB/15.87kB
f67191ae09b8: Loading layer  11.78kB/11.78kB
68dda0c9a8cd: Loading layer  3.072kB/3.072kB
37ff0418e4a3: Loading layer  4.096kB/4.096kB
7d8c7e726e3a: Loading layer  862.2MB/862.2MB
b68bd7d0db1d: Loading layer  4.608kB/4.608kB
c2f6bd4a2e6e: Loading layer  2.048kB/2.048kB
1bb26f8d58f4: Loading layer  305.3MB/689MB
Error processing tar file(exit status 1): write /usr/local/lib/python3.5/dist-packages/scipy/interpolate/dfitpack.cpython-35m-x86_64-linux-gnu.so: no space left on device
```

`rknn-toolkit-1.0.0-docker.tar.gz  2G`

解决方法：

把本地镜像删除一部分

### Docker: no space left on device

``` text
Starting 0912_drone-server_1 ... error

ERROR: for 0912_drone-server_1  Cannot start service drone-server: b'OCI runtime create failed: container_linux.go:348: starting container process caused "process_linux.go:402: container init caused \\"read init-p: connection reset by peer\\"": unknown'

ERROR: for drone-server  Cannot start service drone-server: b'OCI runtime create failed: container_linux.go:348: starting container process caused "process_linux.go:402: container init caused \\"read init-p: connection reset by peer\\"": unknown'
ERROR: Encountered errors while bringing up the project.
```

这种情况在删除了一个正在运行的容器后，得到解决，可能是运行中的容器占用空间过大导致，同时还有一个现象是，在终端操作一些基础命令时，是不是也会提示 `-bash: fork: No space left on device`

Update:

``` text
me@linux:$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
d1725b59e92d: Pull complete
Digest: sha256:523e382ab1801f2a616239b1052bb7ee5a7cce6a06cfed27ccb93680eacad6ef
Status: Downloaded newer image for hello-world:latest
docker: Error response from daemon: OCI runtime create failed: container_linux.go:348: starting container process caused "process_linux.go:301: running exec setns process for init caused \"exit status 46\"": unknown.
ERRO[0015] error waiting for container: context canceled
```

上面的解决方法失效，在测试运行了一个 hello-world 程序之后，依然爆出同样的问题，可能是 docker 启动容器时出现异常

解决方法：

``` url
https://github.com/NVIDIA/nvidia-docker/issues/683
```

可能是由于本机装的 nvidia-docker 导致的，在按照链接的中的方法重新安装 nvidia-docker 后问题得到解决：

``` text
# 可能是nvidia driver/cuda/nvidia-docker2之间出了问题
# 将三者彻底删除
apt-get purge nvidia driver-* cuda-* nvidia-docker2
apt-get update
# 重新安装cuda-9-0，会自动安装相应的nvidia driver-384 就用默认的版本
# 测试 nvidia-smi 通过
# 重新安装 nvidia-docker2
# 测试
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```
