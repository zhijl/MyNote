## 安装 ssh 服务器

``` shell
sudo apt install openssh
```

## SSH root 用户远程登录

``` shell
sudo -s
# 设置 root 用户密码
sudo passwd root
```

修改配置项：

``` shell
vim /etc/ssh/sshd_config
```

将 PermitRootLogin 一项改为：

``` text
PermitRootLogin yes
```

重启 ssh

``` shell
sudo service ssh restart
```