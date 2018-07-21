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

通过进程名杀死进程

``` shell
ps -ef | grep procedure_name | grep -v grep | awk '{print $2}' | xargs kill -9
```

Linux 查看进程资源占用

``` shell
ps -e -o 'pid,comm,args,pcpu,rsz,vsz,stime,user,uid' | grep oracle |  sort -nrk5
```

Linux 查看某一目录下磁盘占用

``` shell
du -ah --max-depth=1 dirname # a 表示目录下及子目录下所有文件；h 表示human readale；max-depth表示目录深度；s 表示只显示该目录下的磁盘占用大小
```

1. [查看LINUX进程内存占用情况](https://www.cnblogs.com/gaojun/p/3406096.html)
