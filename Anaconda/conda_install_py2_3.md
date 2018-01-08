# Anaconda环境中安装Python

> 也即使用conda管理Python

注：Windows平台如果在安装Anaconda时没有勾选**将Anaconda路径添加到PATH环境变量中**的相关选项，那么，要使用conda需要启动**Anaconda Prompt**进行操作

## conda 常用命令

* 查看当前系统下环境

```
conda info -e
```

* 例子

```
(D:\ProgramData\Anaconda2) C:\Users\ASUS>conda info -e
# conda environments:
#
rstudio                  D:\ProgramData\Anaconda2\envs\rstudio
root                  *  D:\ProgramData\Anaconda2
```

* 创建新的环境

```
# 指定python版本为2.7，注意至少需要指定python版本或者要安装的包
# 后一种情况下，自动安装最新python版本
# env_name 是自定义的环境名
conda create -n env_name python=2.7
# 同时安装必要的包
conda create -n env_name numpy matplotlib python=2.7
```

* 例子

```
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

* 环境切换

```
# 切换到新环境
# linux/Mac下需要使用source activate env_name
activate env_name
# 退出环境，也可以使用`activate root`切回root环境
deactivate env_name
```
* 例子
```
(D:\ProgramData\Anaconda2) C:\Users\ASUS>activate zhi_env

(zhi_env) C:\Users\ASUS>
```

* 删除环境

```
conda remove -n env_name --all
```

* 例子

```
(D:\ProgramData\Anaconda2) C:\Users\ASUS>conda remove -n zhi_env --all

Remove all packages in environment D:\ProgramData\Anaconda2\envs\zhi_env:

Proceed ([y]/n)? y
```

## 包管理

> 给某个特定环境安装package有两个选择，一是切换到该环境下直接安装，二是安装时指定环境参数-n

```
activate env_nameconda install pandas
# 安装anaconda发行版中所有的包
conda install anaconda
```

```
conda install -n env_name pandas
```

* 查看已经安装的包
```
conda list
# 指定查看某环境下安装的package
conda list -n env_name
```

* 查找包
```
conda search pyqtgraph
```

* 更新包
```
conda update numpy
conda update anaconda
```

* 卸载包
```
conda remove numpy
```

## 设置系统镜像

可选用[清华大学TUNA镜像](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

```
# 需要去掉网址的引号
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/conda config --set show_channel_urls yes
```

如果命令行方法添加不上，可以在用户目录下的.condarc中添加

文件内容如下：
```
channels:
 - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/ 
 - defaults
show_channel_urls: yes
```


## 参考

[使用conda管理python环境](https://zhuanlan.zhihu.com/p/22678445)