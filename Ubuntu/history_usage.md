# 清楚终端上的历史操作记录

查看 `history` 用法

``` shell
help history
```

## 完全清楚

一般我们已知的是history -c 命令，即清除所有历史记录
但是如果服务器用的是公司的，就不好执行这种粗暴的操作了。。

## 部分删除

- vim ~/.bash_history，该文件即为历史记录存储文件，我们随意修改
- 修改后再次 history 查看，发现并没有变化。原因：缓存，执行：`history -r` 读取历史文件并将其内容添加到历史记录中，即重置文件里的内容到内存中，完成修改

摘自：

- Linux下部分删除history记录 https://blog.csdn.net/Abysscarry/article/details/79700293
