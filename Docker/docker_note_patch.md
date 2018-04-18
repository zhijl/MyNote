# Docker 零散笔记

## 重要概念

- 容器
- 镜像
- 分层存储
- 数据卷

## 启动一个容器

``` sh
docker run            # 后面通常用到的参数
-it                   # 开启模式为可输入终端
--rm                  # 容器退出后删除
-d                    # 将容器启动为后台服务
-p 8080:8080          # 端口映射
--mount source=A,target=B     # 容器内将外部环境中的A目录挂载在容器内的B目录上
# 最后面接上要启动的镜像名称或ID
```

使用 `-v` 参数挂载一个数据卷

``` sh
docker run            # 后面通常用到的参数
-it                   # 开启模式为可输入终端
--rm                  # 容器退出后删除
-d                    # 将容器启动为后台服务
-p 8080:8080          # 端口映射
-v host_dir:inside_dir # 目录映射
```

## 构建镜像

``` shell
docker build
-t my_docker_name:version_num        # 制定镜像名
<Dockerfile 路径>
```

## Docker 去除 sudo

``` shell
# 添加一个新的docker用户组
sudo groupadd docker
# 添加当前用户到docker用户组里，注意这里的yongboy为ubuntu server登录用户名
sudo gpasswd -a yongboy docker
# 重启Docker后台监护进程
sudo service docker restart
# 重启之后，尝试一下，是否生效
docker version
# 若还未生效，则系统重启或注销当前账号再登录，则生效
sudo reboot
```

## 使用 `docker exec -it` 命令进入容器

例如：

``` shell
docker exec -it name /bin/bash
```

也可以通过 touch 参数进行入容器终端，但这种方式不好，不常用，在此不说明

## Ubuntu 中 Docker 容器 Volume 路径

```
/var/lib/docker/volumes
```

## 参考

1. [Docker学习笔记之一，搭建一个JAVA Tomcat运行环境](http://www.blogjava.net/yongboy/archive/2013/12/12/407498.html)
