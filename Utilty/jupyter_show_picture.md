# 在 Jupyter Notebook 中显示图片

> 在 Jupyter Notebook 中显示的本地图片的方法有很多，这里用 PIL 和 Matplotlib 实现

``` python
import os
from PIL import Image
import matplotlib.pyplot as plt

%matplotlib inline

img = Image.open(os.path.join('images', '2007_000648' + '.jpg'))

plt.figure("Image") # 图像窗口名称
plt.imshow(img)
plt.axis('on')      # 关掉坐标轴为 off
plt.title('image')  # 图像题目
plt.show()
```

参考：

1. [python matplotlib 显示图像](https://blog.csdn.net/majinlei121/article/details/78935083)