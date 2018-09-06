# 将图片裁剪成指定大小

用途是在将 112x112 的人脸图片裁剪为 112x96

``` python
from os.path import isfile, join, exists
import os
import cv2

src_file = '011.jpg'
dst_path = 'output'
src_image = cv2.imread(src_file)
crop_img = src_image[:112, 8:104]
if exists(dst_path) == False:
    os.mkdir(dst_path)
dst_file = join(dst_path, src_file)
cv2.imwrite(dst_file, crop_img)
```