# 命令行 wget 下载百度云盘开放资源

1. 获取百度云盘资源共享链接（在百度云盘中把资源共享出来）

如：https://pan.baidu.com/s/1khApxhnPV9uE2msdkDEtdQ

2. 获取资源实际下载路径

点击网页上的下载，在下载管理页面可获取资源的实际下载连接

3. 使用 wget 下载，格式如下：

``` shell
wget -c --referer=百度云盘资源共享连接 -O 指定输出文件名 "百度云盘资源实际下载连接"
```

此处 `-c` 为断点续传

例如：

``` shell
wget -c --referer=http://pan.baidu.com/s/1o6rdkW6 -O windows7_x86.iso "http://nj02all02.baidupcs.com/file/3be75df53e0cfb3905af0b4f4471c9f3?bkt=p-f451872af08cc5bfb490508b523c8397&fid=4227933273-250528-4076405795&time=1468566284&sign=FDTAXGERBH-DCb740ccc5511e5e8fedcff06b081203-vFS%2BBH0fTd3TSGEMc4O%2FTySLxHc%3D&to=nj2vb&fm=Nan,B,T,t&sta_dx=2484&sta_cs=-19281&sta_ft=iso&sta_ct=7&fm2=Nanjing,B,T,t&newver=1&newfm=1&secfm=1&flow_ver=3&pkey=14003be75df53e0cfb3905af0b4f4471c9f3e6fdf91000009b398800&expires=8h&rt=sh&r=706431293&mlogid=4560365228769428710&vuk=4227933273&vbdid=2129571343&fin=cn_windows_7_x86.iso&fn=cn_windows_7_x86.iso&uta=0&rtype=0&iv=2&isw=0&dp-logid=4560365228903646438&dp-callid=0.1"
```



#### 参考

1. [linux 命令行使用wget下载百度云资源](https://blog.csdn.net/zhongdajiajiao/article/details/51917886)