# tqdm 库

> tqdm （taqadum）在阿拉伯语中是进展的意思，库的功能主要是为长循环在终端添加一个进度提示信息输出，也即进度条。用法只需要用 `tqdm` 包上一个迭代器（iterable）即可。

``` python
import time
from tqdm import tqdm
for i in tqdm(range(10000)):  # 也可以用 for i in trange(10000): trange 是 tqdm 的特殊实例
    time.sleep(.01)
    ...
```

还可以手动更新

``` python
with tqdm(total=100) as pbar:
    for i in range(10):
        pbar.update(10)
```

加入描述

``` python
pbar = tqdm(["a", "b", "c", "d"])
for char in pbar:
    pbar.set_description("Processing %s" % char)
```

或者

``` python
pbar = tqdm(total=100)
for i in range(10):
    pbar.update(10)
pbar.close()
```

作为脚本在 shell 中使用 tqdm

``` shell
# 统计所有 python 代码行数

time find . -name '*.py' -exec cat \{} \; | wc -l

find . -name '*.py' -exec cat \{} \; |
    tqdm --unit loc --unit_scale --total 857366 >> /dev/null
```

非强制性依赖的方式加入 tqdm

``` python
# How to import tqdm without enforcing it as a dependency
try:
    from tqdm import tqdm
except ImportError:
    def tqdm(*args, **kwargs):
        if args:
            return args[0]
        return kwargs.get('iterable', None)
```

还有其他更多的一些方法和参数，可参考1.官方文档描述

参考：

1. [Python 进度条 tqdm](https://blog.csdn.net/jiandanjinxin/article/details/72457554)
1. [python的Tqdm模块](https://blog.csdn.net/langb2014/article/details/54798823?locationnum=8&fps=1)
1. [tqdm](https://pypi.org/project/tqdm/)