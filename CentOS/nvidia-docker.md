# CentOS 7 安装 nvidia-docker

按照官网的教程 https://github.com/NVIDIA/nvidia-docker/tree/master#upgrading-with-nvidia-docker2-deprecated 安装 nvidia-docker 时报错

错误信息：

``` text
https://nvidia.github.io/libnvidia-container/stable/centos7/x86_64/repodata/repomd.xml: [Errno -1] repomd.xml signature could not be verified for libnvidia-container
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
https://nvidia.github.io/libnvidia-container/stable/centos7/x86_64/repodata/repomd.xml: [Errno -1] repomd.xml signature could not be verified for libnvidia-container
```

## 解决方法一

参考 [https://blog.csdn.net/u012560213/article/details/101430549](https://blog.csdn.net/u012560213/article/details/101430549)

编辑  /etc/yum.repos.d/nvidia-*.repo 文件

将 repo_gpgcheck=1 修改为 repo_gpgcheck=0

## 解决方法二

最佳的解决方法是更新 gpg key 值

即：

``` sh
DIST=$(sed -n 's/releasever=//p' /etc/yum.conf)
DIST=${DIST:-$(. /etc/os-release; echo $VERSION_ID)}
sudo rpm -e gpg-pubkey-f796ecb0
sudo gpg --homedir /var/lib/yum/repos/$(uname -m)/$DIST/*/gpgdir --delete-key f796ecb0
sudo gpg --homedir /var/lib/yum/repos/$(uname -m)/latest/nvidia-docker/gpgdir --delete-key f796ecb0
sudo gpg --homedir /var/lib/yum/repos/$(uname -m)/latest/nvidia-container-runtime/gpgdir --delete-key f796ecb0
sudo gpg --homedir /var/lib/yum/repos/$(uname -m)/latest/libnvidia-container/gpgdir --delete-key f796ecb0
sudo yum update
```

参考 https://github.com/NVIDIA/nvidia-docker/issues/1094 
