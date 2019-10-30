# 64 位系统平台上让 GCC 支持 32 位目标程序编译

如遇到缺少 `sys/cdefs.h` 的情况，输入以下命令安装 C 标准库

``` sh
sudo apt install build-essential libc6-dev libc6-dev-i386
```

如遇到缺少 `bits/c++config.h` 的情况，输入以下命令安装 GCC 编译相关库

``` sh
sudo apt install gcc-4.7-multilib g++-4.7-multilib
```
