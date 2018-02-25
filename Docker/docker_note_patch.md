# Docker 零散笔记

``` sh
docker run            # 后面通常用到的参数
-it                   # 开启模式为可输入终端
--rm                  # 容器退出后删除
-d                    # 将容器启动为后台服务
-p 8080:8080          # 端口映射
--mount source=A,target=B     # 容器内将外部环境中的A目录挂载在容器内的B目录上
```