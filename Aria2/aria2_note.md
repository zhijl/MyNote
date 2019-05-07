# Aria2 下载器使用

### 安装 aria2

Ubuntu 16.04 平台：

``` sh
sudo apt install aria2
```

### 安装 webui-aria2

需要安装 nodejs

``` sh
conda install -c conda-forge nodejs
```

``` sh
git clone https://github.com/ziahamza/webui-aria2.git
```

### 运行

启动 aria2c

``` sh
aria2c --enable-rpc --rpc-listen-all --disable-ipv6 --rpc-secret yourpassword
```

启动 webui-aria2

``` sh
node  node-server.js
```

- 在 Settings -> Connection Settings -> Enter the secret token 填入 `yourpassword`
- Add 中建立下载任务

https://blog.jiangjiaolong.com/debian-aria2-webui.html