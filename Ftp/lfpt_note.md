# 使用 lftp 登录 FTP 服务器

### 登录

``` shell
lftp 用户名:密码@ftp地址:传送端口
```

### 中文乱码问题

``` shell
set ftp:charset gbk
```

### 下载文件

- 设置本地存放路径

```
lcd /home/zhi/a
```

- 下载

```
get test.txt
```

- 批量下载

```
mget *.txt
```

- 断点续传

```
mget -c *.txt
```

允许10个线程并行下载

```
mget -c -n 10 data.txt
```

- 整个目录下载

```
mirror dirname/
```

### 上传文件

```
put test.txt

mput *.txt

mirror -R dirname/
```

### 参考

1. [linux下登录ftp, lftp命令详解](http://www.php.cn/linux-377775.html)