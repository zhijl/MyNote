# 在容器中使用 GPU

NVIDIA 官方提供有 `nvidia-docker` 通过该命令启动容器可以直接使用 GPU，另外也可以通过 `--device` 和 `-v` 将设备驱动和库文件映射到容器镜像内，也可以使用

例如：

``` sh
export DEVICES=$(\ls /dev/nvidia* | xargs -I{} echo '--device {}:{}')
docker run -it --rm $DEVICES -v /usr/lib64/nvidia/:/usr/local/nvidia/lib64 tensorflow/tensorflow:latest-gpu bash
```
