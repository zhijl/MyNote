## pip 更换国内源实现快速下载包

#### 国内可用源：

``` text
http://pypi.douban.com/ 豆瓣
http://pypi.mirrors.ustc.edu.cn/simple/ 中国科学技术大学
```

#### 使用豆瓣源进行Cython包的安装：

``` shell
pip install Cython -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
```

> 这里的 `-i` 后面的这一段参数需要放在最后，否则pip会提示没有 `-i` 这个选项

#### 配置文件的配置方式：

编辑(新增) `~/.pip/pip.conf`

``` shell
[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
```

#### pip install 指定版本

```
pip install tensorflow==1.3 tensorflow-gpu==1.3 -i https://pypi.tuna.tsinghua.edu.cn/simple/  # 采用国内源下载
```

而 conda 下载制定版本为

```
conda install tensorflow=1.3 tensorflow-gpu=1.3
```

#### pip 生成 requirements.txt

```
pip freeze > requirements.txt
```

对应的根据 `requirements.txt` 安装依赖

```
pip install -r requirements.txt
```

#### 参考

1. [修改pip安装源加快python模块安装](http://blog.csdn.net/u012592062/article/details/51966649)