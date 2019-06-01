# 使用 Ubuntu 时遇到的问题

### 终端光标隐藏

不知道为什么终端光标就不见了，但输入如下语句可恢复

```
echo -ne "\033[?25h"
```

### Ubuntu 界面卡死，重启界面

``` sh
Ctrl + Alt + F1
sudo pkill Xorg
```

> 参考: https://blog.csdn.net/happywran/article/details/8204200