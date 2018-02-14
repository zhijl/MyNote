# 安装Ubuntu 16.04 LST

安装的Ubuntu镜像为：

``` text
ubuntu-16.04.3-desktop-amd64.iso
```

## U盘启动

1. 使用 UltraISO 软件打开Ubuntu镜像
1. 工具 -> 写入硬盘映像
1. 选择硬盘驱动器为U盘
1. 点击写入

![图1](/_Resource/ubuntu_install_01.png)
![图2](/_Resource/ubuntu_install_02.png)
![图3](/_Resource/ubuntu_install_03.png)
![图4](/_Resource/ubuntu_install_04.png)
![图5](/_Resource/ubuntu_install_05.png)

## 系统分区方案

### easy 版

划分`/` 和 `swap` 两个分区即可

### general 版

三个分区

1. 挂载点`/`；主分区；安装系统和软件；大小为30G；分区格式为ext4；
1. 挂载点`/home`；逻辑分区；相当于“我的文档”；大小为硬盘剩下的;分区格式ext4；
1. 挂载`swap`；逻辑分区；充当虚拟内存；大小等于内存大小（2G）；分区格式为swap；
1. 挂载`/boot`；引导分区；逻辑分区；大小为200M；分区格式为ext4

## 参考

[Ubuntu分区方案（菜鸟方案、常用方案和进阶方案）](http://blog.csdn.net/alvern_zhang/article/details/48392895)