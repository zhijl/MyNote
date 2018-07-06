# Linux 下判断磁盘是 SSD 还是 HDD 类型

> 比较适用于检查远程服务器的硬盘是否是 SSD，以方便将训练模型数据存储在 SSD 磁盘上。（SSD：Solid-state Drive， HDD：Hard Disk Drive）

- 查看 `/sys/block/*/queue/rotational` 中的返回值（* 处填写设备名称 如sda1），返回1表示磁盘可转动，为 HDD，返回0，不可转动，为 SDD

``` shell
cat /sys/block/*/queue/rotational  # * 处填写设备名称 如sda1
```

- `lsblk -d -o name,rota`，`-d` 表示显示设备名称， `-o` 表示仅显示特定列。1表示磁盘可转动，为 HDD，返回0，不可转动，为 SDD

`
lsblk -d -o name,rota

NAME  ROTA
dfa      0
sda      0
sdb      1
sdc      1
sdd      1
sde      1
sdf      1
sdg      0
sr0      1
loop1    1
`

> 参考： [Linux下判断磁盘是SSD还是HDD的几种方法](https://blog.csdn.net/sch0120/article/details/77725658/)