# 安装 nvidia-docker

**新版的 nvidia-docker2 仅支持 docker 19.03 以后的版本**

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

### CentOS 7 安装时遇到的问题

**CentOS 7 (docker-ce), RHEL 7.4/7.5 (docker-ce), Amazon Linux 1/2**

```
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.repo | sudo tee /etc/yum.repos.d/nvidia-docker.repo

$ sudo yum install -y nvidia-container-toolkit
$ sudo systemctl restart docker
```

执行 `sudo yum install -y nvidia-container-toolkit` 时报如下错误：

```
...
https://nvidia.github.io/libnvidia-container/centos7/x86_64/repodata/repomd.xml: [Errno -1] pygpgme is not working so repomd.xml can not be verified for libnvidia-container
Trying other mirror.


 One of the configured repositories failed (libnvidia-container),
 and yum doesn't have enough cached data to continue. At this point the only
 safe thing yum can do is fail. There are a few ways to work "fix" this:

     1. Contact the upstream for the repository and get them to fix the problem.

     2. Reconfigure the baseurl/etc. for the repository, to point to a working
        upstream. This is most often useful if you are using a newer
        distribution release than is supported by the repository (and the
        packages for the previous distribution release still work).

     3. Run the command with the repository temporarily disabled
            yum --disablerepo=libnvidia-container ...

     4. Disable the repository permanently, so yum won't use it by default. Yum
        will then just ignore the repository until you permanently enable it
        again or use --enablerepo for temporary usage:

            yum-config-manager --disable libnvidia-container
        or
            subscription-manager repos --disable=libnvidia-container

     5. Configure the failing repository to be skipped, if it is unavailable.
        Note that yum will try to contact the repo. when it runs most commands,
        so will have to try and fail each time (and thus. yum will be be much
        slower). If it is a very temporary problem though, this is often a nice
        compromise:

            yum-config-manager --save --setopt=libnvidia-container.skip_if_unavailable=true

failure: repodata/repomd.xml from libnvidia-container: [Errno 256] No more mirrors to try.
https://nvidia.github.io/libnvidia-container/centos7/x86_64/repodata/repomd.xml: [Errno -1] pygpgme is not working so repomd.xml can not be verified for libnvidia-container
```

解决方法：

```
sudo gpg --homedir /var/lib/yum/repos/x86_64/7/libnvidia-container/gpgdir --delete-key F796ECB0
sudo gpg --homedir /var/lib/yum/repos/x86_64/7/nvidia-container-runtime/gpgdir --delete-key F796ECB0
sudo gpg --homedir /var/lib/yum/repos/x86_64/7/nvidia-docker/gpgdir --delete-key F796ECB0
sudo yum update
```

如果上述删除 key 成功不了，那可以采取下列临时解决方法：

```
Modify /etc/yum.repos.d/nvidia-container-*.repo lines:
repo_gpgcheck=1 -> repo_gpgcheck=0
```

参考：

https://github.com/NVIDIA/nvidia-docker/issues/836