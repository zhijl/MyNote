## Linux 零碎笔记

获取安装软件时的链接信息

``` shell
netstat -an | grep -i ESTABLISHED
```

文件查找

``` shell
find / -name filename.postfix
```

所有环境变量

``` shell
env
```

``` shell
export
```

wget从ftp下载

``` shell
wget ftp://ip:port/software/os/ubuntu12.04/ubuntu-12.04.1-server-amd64.iso --ftp-user=username --ftp-password=password
```