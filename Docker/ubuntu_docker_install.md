# Ubuntu 16.04 安装 Docker

## Docker 支持的 Ubuntu 系统

- Ubuntu 16.04
- Ubuntu 14.04
- Ubuntu 12.04

## 配置需求

要求64为系统,内核至少3.10. 查看内核版本命令:

``` shell
uname -r
```

## 使用脚本安装 Docker

获取最新的 Docker 安装包,终端输入如下语句,然后输入密码,通过脚本进行下载和安装

``` shell
zhi@abc:~$ wget -qO- https://get.docker.com/ | sh
```

输出

``` text
# Executing docker install script, commit: 1d31602
+ sudo -E sh -c apt-get update -qq >/dev/null
[sudo] password for zhi: 
+ sudo -E sh -c apt-get install -y -qq apt-transport-https ca-certificates curl >/dev/null
+ sudo -E sh -c curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add -qq - >/dev/null
+ sudo -E sh -c echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial edge" > /etc/apt/sources.list.d/docker.list
+ [ ubuntu = debian ]
+ sudo -E sh -c apt-get update -qq >/dev/null
+ sudo -E sh -c apt-get install -y -qq --no-install-recommends docker-ce >/dev/null
+ sudo -E sh -c docker version
[sudo] password for zhi: 
Client:
 Version:	18.01.0-ce
 API version:	1.35
 Go version:	go1.9.2
 Git commit:	03596f5
 Built:	Wed Jan 10 20:11:05 2018
 OS/Arch:	linux/amd64
 Experimental:	false
 Orchestrator:	swarm

Server:
 Engine:
  Version:	18.01.0-ce
  API version:	1.35 (minimum version 1.12)
  Go version:	go1.9.2
  Git commit:	03596f5
  Built:	Wed Jan 10 20:09:37 2018
  OS/Arch:	linux/amd64
  Experimental:	false
If you would like to use Docker as a non-root user, you should now consider
adding your user to the "docker" group with something like:

  sudo usermod -aG docker zhi

Remember that you will have to log out and back in for this to take effect!

WARNING: Adding a user to the "docker" group will grant the ability to run
         containers which can be used to obtain root privileges on the
         docker host.
         Refer to https://docs.docker.com/engine/security/security/#docker-daemon-attack-surface
         for more information.
```

后面提示说要以非root用户直接执行 Docker 时,需执行  `sudo usermod -aG docker zhi` 语句,然后在重新登录

注: 这种安装的方式比较耗时,且安装过程不透明

## 启动 Docker 后台服务

``` shell
sudo service docker start
```

## 镜像加速

国内网络问题，后续拉取 Docker 镜像十分缓慢，可以配置加速器来解决，例如网易的镜像地址：

``` text
http://hub-mirror.c.163.com
```

修改 /etc/docker/daemon.json 文件,在其中加入

``` json
{
    "registry-mirrors": [
        "http://hub-mirror.c.163.com"
    ]
}
```

或者使用 docker 官方镜像地址：

``` json
{
    "registry-mirrors": [
        "https://registry.docker-cn.com"
    ]
}
```

[Ubuntu Docker 安装](http://www.runoob.com/docker/ubuntu-docker-install.html)