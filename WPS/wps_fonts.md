
# Ubuntu平台WPS字体确实问题

-------------------------

## 原因

WPS for Linux没有自带windows的字体，只要在Linux系统中加载字体即可。

## 步骤如下

1.   载缺失的字体文件，然后复制到Linux系统中的/usr/share/fonts文件夹中 
> 国内下载地址：http://pan.baidu.com/s/1mh0lcbY

2.  下载完成后，解压并进入目录中

```
sudo cp * /usr/share/fonts
```

3.  执行以下命令,生成字体的索引信息

```
sudo mkfontscale
sudo mkfontdir
```
4.  运行fc-cache命令更新字体缓存

```
sudo fc-cache
```

5.  重启WPS，看看是否还有字体缺失提示出现

---------------------