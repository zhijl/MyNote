## Docker 清理工作

查看磁盘占用情况

``` sh
docker system df
```

``` text
TYPE                TOTAL               ACTIVE              SIZE                RECLAIMABLE
Images              16                  5                   14.28GB             12.84GB (89%)
Containers          5                   5                   104.4MB             0B (0%)
Local Volumes       18                  5                   1.288GB             839.6MB (65%)
Build Cache                                                 0B                  0B
```

清理磁盘，包括删除关闭的容器、无用的数据卷和网络，已经 dangling 镜像（无tag镜像）

``` sh
docker system prune
```

加 `-a` 会使所有没被容器使用的镜像删除

``` sh
docker system prune -a
```

Ref: http://dockone.io/article/3056