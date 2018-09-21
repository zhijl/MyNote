# Linux adb 连接设备 shell

## USB 连接

### 查看设备 id

使用 `lsusb` 查看 usb 设备（插入 usb 和 拔出 usb 观察 lsusb 中 新增的设备）

``` sh
lsusb

Bus 002 Device 002: ID 8087:8000 Intel Corp.
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:8008 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 002: ID 0461:4e04 Primax Electronics, Ltd
Bus 003 Device 011: ID 2717:9039
Bus 003 Device 003: ID 17ef:6019 Lenovo
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

如上，记住 `2717:9039`

### 添加权限

``` sh
vim /etc/udev/rules.d/70-android.rules
```

加入内容：

``` text
SUBSYSTEM=="usb", ATTRS{idVendor}=="2717", ATTRS{idProduct}=="9039", MODE="0666"
```

`idVendor` 和 `idProduct` 分别为上面的 `2717:9039`

### 重启服务

重启 usb 服务

``` sh
sudo chmod a+rx /etc/udev/rules.d/70-android.rules
sudo service udev restart
```

重启 adb 服务

``` sh
adb kill-server
adb start-server
```

## 网线连接

``` sh
adb connect 192.168.1.120
```

## 尝试重启 adb

``` sh
adb kill-server
adb start-server
```

参考：

https://blog.csdn.net/leokelly001/article/details/43485691