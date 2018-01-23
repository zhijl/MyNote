# Ubuntu 添加用户

## 添加用户的指令

可以使用两个指令 `adduser` 和 `useradd` ，区别在于

- `adduser name` 将在/home目录下创建name文件夹
- `useradd name` 不会在/home目录下创建name文件夹
- `useradd -m name` 会在/home目录下创建name文件夹，此时效果同 `adduser name`

## 参考

[1] [ubuntu用户添加adduser, useradd并给予sudo权限](http://blog.csdn.net/u011534057/article/details/51679358)