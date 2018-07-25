# 生成图片类别标签

目录格式为：

``` text
# 每个目录为一个类别
train_root
├── 0001
│   ├── 01.jpg
│   ├── 02.jpg
│   └── ...
├── 0002
│   ├── 01.jpg
│   ├── 02.jpg
│   └── ...
├── 0003
│   ├── 01.jpg
│   ├── 02.jpg
│   └── ...
├── 0004
│   ├── 01.jpg
│   ├── 02.jpg
│   └── ...
...
```

使用 Python 代码生成类别标签：

``` python
#-*- coding: utf-8 -*-

from os.path import isfile, join
from os import listdir
import time

if __name__ == '__main__':
  clock_start = time.time()

  # root path of train data
  root_path = '/public/Data04/VGGFace2_crop/train'
  label_file_path = '/public/Data04/VGGFace2_crop/train_label.txt'

  subpaths = listdir(root_path)

  label_id = 0  # 类别起始编号
  with open(label_file_path, 'w+') as f:
    for sub in subpaths:
      for file_name in listdir(join(root_path, sub)):
        if (file_name.find("fea") != -1):  # 过滤其他文件
          continue
        file_path = join(root_path, sub, file_name)
        print file_path
        if (isfile(file_path)):
          f.writelines(file_path + ' ' + str(label_id) + '\n')
      label_id = label_id + 1

  clock_end = time.time()
```