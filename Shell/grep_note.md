## grep 命令笔记

### 查找某一目录下包含某一字符串的文件

``` shell
# -r 表示递归子目录
# -i 表示忽略大小写
# -n 表示显示行数
# -l 表示只显示匹配文件名
grep -rin "hello" /home/test
```

一个复杂的例子，目标是从Jenkins系统目录下取得所有代理节点的标签名

``` shell
cd /var/jenkins_home/nodes/ && grep -rn "<label>" | awk '{print $2}' | awk -v FS="<label>" -v OPS=" " '{print $2}' | awk -v FS="</label>" -v OFS=" " '{print $1}' | tr "\n" " " | awk -v OFS="\\\n" '{$1=$1; print}'
```