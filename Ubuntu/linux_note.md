## Linux 零碎笔记

清空 shell 屏幕日志

``` sh
reset  # 会清空前面设置的变量

printf "\033c"  # 推荐使用这个
```

Mac 下使用 "command + k" 即可

---

创建硬/软链接

硬链接即一个指针，指向文件索引节点，系统并不为它重新分配 inode

``` sh
ln abc new_abc  # 建立 abc 的硬链接
```

软链接（符号链接），软链接克服了硬链接的不足，没有任何文件系统的限制，任何用户可以创建指向目录的符号链接。因而现在更为广泛使用，它具有更大的灵活性，甚至可以跨越不同机器、不同网络对文件进行链接

``` sh
ln -s abc new_abc # 建立 abc 的软链接，其他选项 -f 不管 new_abc 存不存在，都创建  -n 如果 new_abc 不存在，则不创建
```

---

挂载/卸载硬盘

``` sh
sudo fdisk -l   # 列出系统中连接的硬盘信息
```

``` sh
sudo blkid  # 查看磁盘分区 UUID
```

``` sh
# 挂载分区
mount /dev/sdb1 /data1
```

配置开机自动挂载

``` sh
vim /etc/fstab

# 加入：
UUID=11263962-9715-473f-9421-0b604e895aaa /data ext4 defaults 0 1

# 注：
<fs spec> <fs file> <fs vfstype> <fs mntops> <fs freq> <fs passno>
具体说明，以挂载/dev/sdb1为例:
<fs spec> :
分区定位，可以给UUID或LABEL，例如：UUID=6E9ADAC29ADA85CD或LABEL=software
<fs file> : 具体挂载点的位置，例如：/data
<fs vfstype> : 挂载磁盘类型，linux分区一般为ext4，windows分区一般为ntfs
<fs mntops> : 挂载参数，一般为defaults
<fs freq> : 磁盘检查，默认为0
<fs passno> : 磁盘检查，默认为0,不需要检查
```

修改完 `/etc/fstab` 文件后，运行 `sudo mount -a` 命令验证一下配置是否正确

---

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

ubuntu 删除 repository ppa

```
sudo add-apt-repository --remove ppa:tboox/xmake
```


1. [查看LINUX进程内存占用情况](https://www.cnblogs.com/gaojun/p/3406096.html)
