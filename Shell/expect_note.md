# Expect Linux自动交互工具

## 简介

Expect是一个用来处理交互的命令。借助Expect，我们可以将交互过程写在一个脚本上，使之自动化完成。Expect建立在Tcl基础上实现。

Ubuntu 16.04 平台下安装 `sudo apt install except`

## 关键命令

* `spawn` 启动新的进程

`spawn` 打开一个回话（进程），然后通过 `expect` 和 `send` 和该进程进行交互。

``` shell
# 格式
spawn ${cmd}
# 例如
spawn su root
```

* `set` 设置变量

``` shell
set param "Hello"
set param_int 100
# 另外，设置timeout可指定会话时间在一定范围内，如果不限制，可设置为-1
set timeout 30
```

* `expect` 从进程接受字符串

`expect` 接收与命令执行后的输出，并能与预期的字符串进行匹配，如果满足匹配，则可通过 `send` 发送相应字符串

`expect eof` 声明结束等待，完成交互

``` shell
expect "$case1" { send "$respond1\r" }
# 多分支
expect {
    "case1" { send "$respond1\r" }
    "case2" { send "$respond2\r" }
    "case3" { send "$respond3\r" }
}
```

> [注] 转义字符`\r`直接回到本行的起始位置，可能会覆盖掉原有的已经输出的字符；而`\b`只回到它前一个字符的位置，而且也没有删除回退过的字符。

* `send` 向进程发送字符串

例子见上面 `expect`

* `interact` 允许用户交互

如果在完成前面的自动交互后，还需要一些额外的手动交互，可通过 `interace` 直接声明

`expect eof` 和 `interact` 二选一

## 输入参数

输入参数存放在 `argv` 变量中，变量 `argc` 存放参数个数，使用如下格式依次获取每个参数

``` shell
[lindex $argv 0]
[lindex $argv 1]
[lindex $argv 2]
...
```

## 例子

照搬参考文献 [1.](#1)

``` shell
#!/usr/bin/expect

if {$argc < 4} {
    #do something
    send_user "usage: $argv0 <remote_user> <remote_host> <remote_pwd> <remote_root_pwd>"
    exit
}

set timeout -1
set remote_user [lindex $argv 0] # 远程服务器用户名
set remote_host [lindex $argv 1] # 远程服务器域名
set remote_pwd [lindex $argv 2] # 远程服务器密码
set remote_root_pwd [lindex $argv 3] # 远程服务器根用户密码

# 远程登录
spawn ssh ${remote_user}@${remote_host}
expect "*assword:" {send "${remote_pwd}\r"}
expect "Last login:"

# 切换到 root
send "su\r"
expect "*assword:" {send "${remote_root_pwd}\r"}

# 执行关闭防火墙命令
send "service iptables stop\r"
send "exit\r"
send "exit\r"
expect eof
```

执行方式

``` shell
chmod 777 remote_root_command.exp
./remote_root_command.exp <remote_user> <remote_host> <remote_pwd> <remote_root_pwd>
```

## 参考

<span id=1>1. [每次进步一点点——linux expect 使用](http://blog.csdn.net/houmou/article/details/53102051)</span>