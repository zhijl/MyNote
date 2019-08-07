# 安装 nvidia-docker

新版的 nvidia-docker2 仅支持 docker 19.03 以后的版本

`from https://github.com/NVIDIA/nvidia-docker`

### Ubuntu 16.04/18.04, Debian Jessie/Stretch

**Add the package repositories**

```
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

$ sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
$ sudo systemctl restart docker
```

执行第三条指令时报错

```
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

报错：

```
...
AppStream system cache was updated, but problems were found: Metadata files have errors: /var/cache/app-info/xmls/fwupd.xml
Reading package lists... Done
E: Problem executing scripts APT::Update::Post-Invoke-Success 'if /usr/bin/test -w /var/cache/app-info -a -e /usr/bin/appstreamcli; then appstreamcli refresh-cache > /dev/null; fi'
E: Sub-process returned an error code
```

解决方法：

```
# sudo rm /var/cache/app-info/xmls/fwupd.xml
# sudo appstreamcli refresh --force
AppStream cache update completed successfully.
# sudo apt update
```

参考：

https://askubuntu.com/questions/1053869/apt-update-error-from-fwupd-xml/1060895