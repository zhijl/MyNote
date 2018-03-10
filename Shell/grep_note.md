## grep 命令笔记

### 查找某一目录下包含某一字符串的文件

``` shell
# -r 表示递归子目录
# -i 表示忽略大小写
# -n 表示显示行数
# -l 表示只显示匹配文件名
grep -rin "hello" /home/test
```

