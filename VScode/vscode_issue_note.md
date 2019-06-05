# VS Code 问题记录

如果 VS Code 启动时内存吃紧，可能是 `max_user_watches` 设置过大

``` sh
cat /proc/sys/fs/inotify/max_user_watches
```

改小改值如下：

``` sh
sudo vim /etc/sysctl.conf

# 修改：
fs.inotify.max_user_watches = 8192

# 使配置文件生效
sudo sysctl -p
```
