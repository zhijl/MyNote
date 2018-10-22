# 同时按相同顺序打乱两个列表

``` python
import random

aa = [1, 2, 3, 4, 5, 6]
bb = [1, 2, 3, 4, 5, 6]

cc = list(zip(aa, bb))
random.shuffle(cc)
aa[:], bb[:] = zip(*cc)
print(aa, bb)
```

ref： https://blog.csdn.net/yideqianfenzhiyi/article/details/79197570