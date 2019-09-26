# SSH 设置别名登录服务器

一般 `~/.ssh` 目录下会存在 `config`、`known_hosts` 两个文件，我们需要在 `config` 文件中进行配置，如果没有则创建一个

配置如下：

``` text
Host zhi-server
HostName 192.168.1.34
user root
IdentitiesOnly yes
```

- Host 别名
- HostNanme 主机 IP
- User 登录用户名
- IdentitiesOnly yes 表示固有配置

配置完成后：

``` sh
ssh zhi-server
```
