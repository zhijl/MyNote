# Anaconda环境中安装Python

> 也即使用conda管理Python

注：Windows平台如果在安装Anaconda时没有勾选**将Anaconda路径添加到PATH环境变量中**的相关选项，那么，要使用conda需要启动**Anaconda Prompt**进行操作

## conda 常用命令

* 查看当前环境安装的包

``` shell
conda list
```

* 检查更新 conda

``` shell
conda update conda
```

* 查看当前系统下已有环境

``` shell
conda info -e
# 或
conda info --envs
```

``` shell
conda env list
```

* 例子

``` shell
(D:\ProgramData\Anaconda2) C:\Users\ASUS>conda info -e
# conda environments:
#
rstudio                  D:\ProgramData\Anaconda2\envs\rstudio
root                  *  D:\ProgramData\Anaconda2
```

* 创建新的环境

``` shell
# 指定python版本为2.7，注意至少需要指定python版本或者要安装的包
# 后一种情况下，自动安装最新python版本
# env_name 是自定义的环境名
conda create -n env_name python=2.7
# 同时安装必要的包
conda create -n env_name numpy matplotlib python=2.7
```

* 例子

``` shell
(D:\ProgramData\Anaconda2) C:\Users\ASUS>conda create -n zhi_env
Fetching package metadata .............
Solving package specifications:
Package plan for installation in environment D:\ProgramData\Anaconda2\envs\zhi_e
nv:

Proceed ([y]/n)? y

#
# To activate this environment, use:
# > activate zhi_env
#
# To deactivate an active environment, use:
# > deactivate
#
# * for power-users using bash, you must source
#
```

* 复制某个环境

``` shell
conda create --name new_env_name --clone old_env_name
```

* 删除环境

``` shell
conda remove -n env_name --all
```

* 例子

``` shell
(D:\ProgramData\Anaconda2) C:\Users\ASUS>conda remove -n zhi_env --all

Remove all packages in environment D:\ProgramData\Anaconda2\envs\zhi_env:

Proceed ([y]/n)? y
```

* 环境切换

``` shell
# 切换到新环境
# linux/Mac下需要使用source activate env_name
activate env_name
# 退出环境，也可以使用`activate root`切回root环境
deactivate env_name
```

* 例子

``` shell
(D:\ProgramData\Anaconda2) C:\Users\ASUS>activate zhi_env

(zhi_env) C:\Users\ASUS>
```

* 分享环境

``` shell
conda env export > environment.yml
```

``` shell
conda env create -f environment.yml
```

## 包管理

> 给某个特定环境安装package有两个选择，一是切换到该环境下直接安装，二是安装时指定环境参数-n

``` shell
activate env_nameconda install pandas  # linux 平台为 source activate env_name
# 安装anaconda发行版中所有的包
conda install anaconda
```

``` shell
conda install -n env_name pandas
```

* 查看已经安装的包

``` shell
conda list
# 指定查看某环境下安装的package
conda list -n env_name
```

* 查找包

``` shell
conda search pyqtgraph
```

* 更新包

``` shell
conda update numpy
conda update anaconda
```

* 卸载包

``` shell
conda remove numpy
```

## 设置系统镜像

可选用[清华大学TUNA镜像](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

``` shell
# 需要去掉网址的引号
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/conda config --set show_channel_urls yes
```

如果命令行方法添加不上，可以在用户目录下的.condarc中添加

文件内容如下：

``` shell
channels:
 - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ 
 - defaults
show_channel_urls: yes
```

### 离线安装 Python 库

比如有时后清华镜像中没有所需的版本库，这是 conda 将从官方站点下载，由于网站在国外，在没有代理的情况下下载速度超级慢，而且经常网络中断，所以可以尝试从浏览器代理的情况下访问官网，下载所需的版本库后，离线安装

conda 资源官网

``` url
https://anaconda.org/anaconda/repo
```

``` shell
conda install ***.tar.bz2
```

## 参考

[使用conda管理python环境](https://zhuanlan.zhihu.com/p/22678445)