# Docker 容器编排工具

## Docker Compose

> Linux 系统需要先安装 Docker，Mac 和 Windows 桌面版内置 Docker Compose

Docker Compose 是一个用来定义和运行复杂应用的 Docker 工具。一个使用 Docker 容器的应用，通常可能由多个容器组成。使用Docker Compose 不再需要使用 shell 脚本来启动容器。

Compose 通过一个配置文件来管理多个 Docker 容器，在配置文件中，所有的容器通过 services 来定义，然后使用docker-compose.yml脚本来启动，停止和重启应用，和应用中的服务以及所有依赖服务的容器，非常适合组合使用多个容器进行开发的场景。

## 安装

```
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

## 参考

1. [Docker Compose](https://docs.docker.com/compose/)
2. [docker-compose教程（安装，使用, 快速入门）](https://blog.csdn.net/pushiqiang/article/details/78682323)