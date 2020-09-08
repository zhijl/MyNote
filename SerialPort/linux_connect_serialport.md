# Linux 系统上通过串口链接设备终端

实践的系统为 Ubuntu 16.04

可行的一些工具有 cu、screen、minicom、putty 等，下面使用 cu 进行尝试

https://blog.csdn.net/zoujiachi666/article/details/79441340

### 安装 cu

Ubuntu 16.04 不带有 cu 工具，需通过 `sudo apt install cu` 进行下载安装

### 连接设备

通过 USB 接口连接设备后，在 /dev/ 目录下会出现 `/dev/ttyUSB0`，通过下面的指令连接设备

``` sh
root@abc:~# cu -l /dev/ttyUSB0 -s 19200
cu: open (/dev/ttyUSB0): Permission denied
cu: /dev/ttyUSB0: Line in use
```

提示 `Permission denied`，修改 `/dev/ttyUSB0` 的读写权限即可，即

``` sh
chmod 666 /dev/ttyUSB0
```

### minicom

minicom 也是一个比较好的工具

`minicom -s` 打开菜单 -> 进行相应的配置 -> 退出配置 即可进入连接

``` sh
minicom -s
```

使用 `minicom` 的过程中可能会遇到这样的问题：

``` text
Device /dev/ttyUSB0 is locked.
```

原因可能是之前使用时没有完全退出 `minicom`，导致进程还在占用串口，解决方法可以尝试杀死进程，即：

``` sh
killall -9 minicom
```
