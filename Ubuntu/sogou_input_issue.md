# 解决 Sogou 输入法崩溃问题

``` sh
#!/bin/sh

pidof fcitx | xargs kill

pidof sogou-qimpanel | xargs kill

nohup fcitx 1>/dev/null 2>/dev/null &

nohup sogou-qimpanel 1>/dev/null 2>/dev/null &

echo "OK"
```

将该脚本拷贝到 /usr/bin 下，当输入法崩溃时，执行该脚本即可