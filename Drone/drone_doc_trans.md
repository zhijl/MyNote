# Drone

### 安装

拉取镜像

``` shell
docker pull drone/drone:0.8
```

使用 `docker-compose` 启动

``` yml
version: '2'

services:
  drone-server:
    image: drone/drone:0.8
    ports:
      - "8000:8000"
      - "9000:9000"
    volumes:
      - ./drone:/var/lib/drone/
    restart: always
    environment:
      - DRONE_OPEN=true
      - DRONE_HOST=http://localhost:8000
      - DRONE_DEBUG=true
      # 随机字符
      - DRONE_SECRET=ALQU2M0KdptXUdTPKcEw
      - DRONE_GOGS=true
      - DRONE_GOGS_URL=${DRONE_GOGS_URL}
      - DRONE_GOGS_SKIP_VERIFY=false

  drone-agent:
    image: drone/agent:0.8
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DRONE_SERVER=drone-server:9000
      - DRONE_SECRET=ALQU2M0KdptXUdTPKcEw
      - DRONE_DEBUG=true

```

``` shell
env DRONE_GOGS_URL=http://192.168.1.111:3000 docker-compose -f drone-deploy.yml up -d
```

注意这里的 `DRONE_GOGS_URL` 需要带 `http`，启动完成后，在浏览器中输入 `drone_ip:8000` 即可访问 drone server，登录密码为 gogs 的用户账号密码

### 基于 Drone 的项目开发流程

- `git push` 提交代码到版本控制系统（GitHub、GitLab、Gogs 等）
- 版本控制系统通过 webhook 触发 Drone 的 pipeline
- Drone 执行 pipeline，build 构建项目
- 构建 Docker 镜像（需要 Dockerfile 文件）
- 将镜像 push 到 registry（Harbor 等）

### 参考：

1. [Drone 中的概念：webhooks、workspace、cloning、pipelines、services、plugins、deployments](https://blog.csdn.net/kikajack/article/details/80503786)