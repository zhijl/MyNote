# Ubuntu 下 ftp 的使用

使用步骤：

1. 连接服务器
1. 目录操作
1. 下载文件
1. 上传文件

## 连接 FTP 服务器

``` shell
ftp domain.com
ftp 192.168.0.120
ftp user@ftpdomain.com
```

如果 ftp 设有密码认证，则会询问用户名和密码，对于匿名 ftp 服务器，可尝试 `anonymous` 作为用户名和使用空密码

## 目录操作

- `ls` 显示目录
- `cd` 进入目录
- `mkdir` 新建目录

与 Linux 操作相同

## 下载文件

- `lcd` 指定下载文件存放位置，如果没有指定，则默认存放在登录 ftp 时的工作目录
- `get` 下载文件
- `mget` 下载多个文件，可使用通配符

``` shell
get fileA
```

## 上传文件

- `put` 上传文件
- `mput` 上传多个文件

``` shell
put fileB            # fileB 在本地目录
put /home/abc/fileC
```

## 关闭 ftp 连接

``` shell
bye
exit
quit
```

## 参考

1. [如何在命令行中使用 ftp 命令上传和下载文件](https://linux.cn/article-6746-1.html)

2018年04月03日14:45:44