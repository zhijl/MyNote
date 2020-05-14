# 记录使用 adb 连接 Android 设备的方式和注意事项

客户端 adb 要与 Android 端的 adbd 进行连接，需先建立通信连接，目前我知道的有三种方式：

- USB 直连设备
- 设备通过网线接入局域网
- 设备通过 WiFi 连接入局域网

这里将记录一些各种连接方式的注意事项和方案：

### USB 直连设备

- Android 端需要 **开启开发者模式** 中的 **USB 调试** 选项

### 网线接入局域网

一般 USB 直连是更好的方式，但有时候 USB 接口出问题等等原因，不能通过 USB 直连，但是设备上提供有 RJ45 网线接口，那么这个方案也可以。但为了保证 `adb connect ip_addr` 成功，需要注意：

1. 本地能 ping 通 Android 设备上的 IP 地址
2. 在 ping 通 IP 的情况下，但是出现这样的问题 `unable to connect to ip_addr:5555`，最可能的原因是 Android 端没有开启 adb 服务，这种情况可以在 Android 端安装一个 Android 虚拟终端，在 Android 虚拟终端下，切换到 root 权限，打开 adb 服务，即

``` sh
su
setprop service.adb.tcp.port 5555
stop adbd
start adbd
```

3. 如果在 Android 端开启了 adb 服务的情况下，还是出现 `unable to connect to ip_addr:5555`，那可再试试用管理员权限重启本地的 adb 服务，即

``` sh
sudo adb kill-server
sudo adb start-server
```

另外 2. 中也可以通过安装类似 SSHDroid 这样的 App，实现 adb 服务的开启，前提是 App 需要获取 Android 系统的 Root 权限。在一次实践中，我尝试过几种类似的 Android ssh 服务端 App，其中就 SSHDroid 能够直接赋予登录的账号 root 权限，这样一来可以直接 scp 拷贝 Android /data 等目录中的数据。另外，有些 Android 系统可能需要通过其他方式破解以获得 Root 权限才可以。正对破解的 Android 或者定制的 Android 系统，可能能在开发者选项中找到 Root 模式的开关。

### WiFi 接入局域网

有时候上述两种接入方式都实现不了，但设备提供了可正常使用的 WiFi，那么值得一式。WiFi 的方式的一个缺陷就是传输速度可能会比较慢。

WiFi 的方式也需要依赖第三方软件，我使用过 `adbWireless` 这款工具，可参考 [Android开发无线调试工具adbwireless的使用简介（附AirADB）](https://www.jianshu.com/p/825e528a8311)

需要注意的是通过连接前需要确保 `adbWireless` 开启
