## AWK 笔记

### 取第一列数据

``` shell
awk '{print $1}' # 取第一列
```

### 例如docker批量删除某一类容器

``` shell
# 删除存在的jenkins容器
linux# docker rm $(docker ps -a | grep jenkins | awk '{print $1}')
```

### AWK 提取 xml 文件某一字段值

``` shell
# 提取<label>value</label>标签的值
awk '/<label>.+<\/label>/' config.xml | awk -v FS="<label>" -v OPS=" " '{print $2}' | awk -v FS="</label>" -v OFS=" " '{print $1}'
```

### AWK 去除文本的第一行

例如提取 docker 镜像列表

``` shell
docker image ls | awk 'NR>2 {print $1":"$2}'
```