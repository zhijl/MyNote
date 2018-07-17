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

``` shel
lcd /home/zhi/a
```

- 下载

``` shell
get test.txt
```

- 批量下载

``` shell
mget *.txt
```

- 断点续传

``` shell
mget -c *.txt
```

允许10个线程并行下载

``` shell
mget -c -n 10 data.txt
```

- 整个目录下载

``` shell
mirror dirname/
```

### 上传文件

``` shell
put test.txt

mput *.txt

mirror -R dirname/
```

### 参考

1. [linux下登录ftp, lftp命令详解](http://www.php.cn/linux-377775.html)