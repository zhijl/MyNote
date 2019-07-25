# 使用 conda 更新当前环境中的 GCC 版本

有些情况下需要使用高版本的 GCC 编译某些项目程序，但是系统中的为低版本（4.8），这时可以使用 conda 配置一个基于当前环境的高版本 GCC，这样既不会影响系统又方便管理

### 离线下载安装

```
https://anaconda.org/psi4/gcc-5/files
```

### 在 Anaconda 的源里查找合适的 GCC

地址：

```
https://anaconda.org/search?q=gcc
```

但是有些库中的 gcc 下载不了，比如 `conda install -c 3dhubs gcc-5`，提示如下错误

```
The following NEW packages will be INSTALLED:

    cloog: 0.18.0-0 http://mirrors.ustc.edu.cn/anaconda/pkgs/free
    gcc-5: 5.2.0-1  3dhubs                                       
    gmp:   6.1.0-0  http://mirrors.ustc.edu.cn/anaconda/pkgs/free
    isl:   0.15-0   http://mirrors.ustc.edu.cn/anaconda/pkgs/free
    mpc:   1.0.3-0  http://mirrors.ustc.edu.cn/anaconda/pkgs/free
    mpfr:  3.1.5-0  http://mirrors.ustc.edu.cn/anaconda/pkgs/free

Proceed ([y]/n)? Invalid choice: conda install -c 3dhubs gcc-5
Proceed ([y]/n)? y

gcc-5-5.2.0-1. 100% |##############################################################################################################| Time: 0:00:44   1.83 MB/s
ERROR conda.core.link:_execute_actions(337): An error occurred while installing package '3dhubs::gcc-5-5.2.0-1'.
LinkError: post-link script failed for package 3dhubs::gcc-5-5.2.0-1
running your command again with `-v` will provide additional information
location of failed script: /home/software/anaconda2/envs/mmdet/bin/.gcc-5-post-link.sh
==> script messages <==
<None>

Attempting to roll back.


LinkError: post-link script failed for package 3dhubs::gcc-5-5.2.0-1
running your command again with `-v` will provide additional information
location of failed script: /home/software/anaconda2/envs/mmdet/bin/.gcc-5-post-link.sh
==> script messages <==
<None>



```

目前没有深知原因，所以可能需要大量尝试吧，如下载 gcc_49 版本

```
conda install -c serge-sans-paille gcc_49   # 亲测有效
```

再将 envs 中的 gcc-4.9 更名为 gcc 即可

参考：

https://www.zhihu.com/question/56272908