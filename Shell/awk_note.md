# AWK 笔记

``` shell
awk '{print $1}' # 取第一列
```

例如docker批量删除某一类容器

``` shell
# 删除存在的jenkins容器
linux# docker rm $(docker ps -a | grep jenkins | awk '{print $1}')
```