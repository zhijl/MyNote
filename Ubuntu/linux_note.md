## Linux 零碎笔记

获取安装软件时的链接信息

rename 可以用于批量修改文件名，perl 语言实现，所以支持正则表达式，但 mv 不能。rename 比较适合用于统一修改文件名的前缀后者后缀

``` sh
# 将当前目录下所有文件名的大写字母修改为小写
rename  'y/A-Z/a-z/'  *
```

``` sh
netstat -an | grep -i ESTABLISHED
```

``` sh
# 为指令设置别名
alias -p  # 查看系统中已经设置的别名
alias ll='ls -l'  # 设置别名
unslias ll # 删除 ll 这个别名
```

文件查找

``` sh
find / -name filename.postfix
```

显示进程树

``` sh
pstree
```

所有环境变量

``` sh
env
```

``` sh
export
```

wget从ftp下载

``` sh
wget ftp://ip:port/software/os/ubuntu12.04/ubuntu-12.04.1-server-amd64.iso --ftp-user=username --ftp-password=password
```

通过进程名杀死进程

``` sh
ps -ef | grep procedure_name | grep -v grep | awk '{print $2}' | xargs kill -9
```

Linux 查看进程资源占用

``` sh
ps -e -o 'pid,comm,args,pcpu,rsz,vsz,stime,user,uid' | grep oracle |  sort -nrk5
```

Linux 查看某一目录下磁盘占用

``` sh
du -ah --max-depth=1 dirname # a 表示目录下及子目录下所有文件；h 表示human readale；max-depth表示目录深度；s 表示只显示该目录下的磁盘占用大小
```

统计当前目录的磁盘占用

``` sh
du -sh
```

1. [查看LINUX进程内存占用情况](https://www.cnblogs.com/gaojun/p/3406096.html)
