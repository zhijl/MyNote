#### 分块划分一个列表

``` python
import math

data = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]
count = len(data)
batch = 10  # 每块 item 个数
iters = int(math.floor(count / batch)) # 可均的 batch 个数
surplus = count % batch
for i in range(iters): # 处理每个batch
    index_start = i * batch
    index_end = index_start + batch
    print(data[index_start:index_end])
if surplus != 0: # 处理末尾剩余的 item
    print(data[iters*batch:iters*batch+surplus])
```
