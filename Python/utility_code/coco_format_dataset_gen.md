# 将个人数据集转为 COCO 格式的数据集

`classes.csv` 类别标签文件
`annos.csv`   注释文件

``` py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

'''
  将数据转为 COCO 数据集格式

root_path
├── annos.csv
├── annotations  # 存放生成的 COCO 格式的 json 文件
├── classes.csv
└── images       # 存放所有图像

'''

import cv2
import json
import os

root_path = '/home/zhi/data-fast/visual_traffic_logos_cocoformat/visual_traffic_logos_train'
phase = 'val'
split_rate = 0.9

# 打开类别标签
with open(os.path.join(root_path, 'classes.csv')) as f:
    classes = f.read().strip().split()

dataset = {}
dataset['categories'] = []

# 建立类别标签和数字id的对应关系
for i, cls_str in enumerate(classes[1:]):
    cls = cls_str.strip().split(',')
    dataset['categories'].append({'id': int(cls[1]), 'name': cls[0], 'supercategory': 'traffic_logo'})

dataset['categories'].append({'id': 0, 'name': 'other', 'supercategory': 'others'})

# 读取images文件夹的图片名称
indexes = [f for f in os.listdir(os.path.join(root_path, 'images'))]

# 判断是建立训练集还是验证集
split_num = int(split_rate * len(indexes))
if phase == 'train':
    indexes = [line for i, line in enumerate(indexes) if i < split_num]
elif phase == 'val':
    indexes = [line for i, line in enumerate(indexes) if i >= split_num]

# 读取Bbox信息
with open(os.path.join(root_path, 'annos.csv')) as tr:
    annos = tr.readlines()

dataset['images'] = []
dataset['annotations'] = []
i = 0
for k, index in enumerate(indexes):
    # 用opencv读取图片，得到图像的宽和高
    im = cv2.imread(os.path.join(root_path, 'images/') + index)
    height, width, _ = im.shape

    # 添加图像的信息到dataset中
    dataset['images'].append({'file_name': index,
                              'id': k,
                              'width': width,
                              'height': height})

    for ii, anno in enumerate(annos):
        parts = anno.strip().split(',')

        # 如果图像的名称和标记的名称对上，则添加标记
        if parts[0] == index:
            # 类别
            cls_id = parts[9]
            # x_min
            x1 = float(parts[1])
            # y_min
            y1 = float(parts[2])
            # x_max
            x2 = float(parts[5])
            # y_max
            y2 = float(parts[6])
            width = max(0, x2 - x1)
            height = max(0, y2 - y1)
            dataset['annotations'].append({
                'area': width * height,
                'bbox': [x1, y1, width, height],
                'category_id': int(cls_id),
                'id': i,
                'image_id': k,
                'iscrowd': 0,
                 # mask, 矩形是从左上角点按顺时针的四个顶点
                'segmentation': [[x1, y1, x2, y1, x2, y2, x1, y2]]
            })
    i = i+1

# 保存结果的文件夹
folder = os.path.join(root_path, 'annotations')
if not os.path.exists(folder):
    os.makedirs(folder)
json_name = os.path.join(root_path, 'annotations/{}.json'.format(phase))
with open(json_name, 'w') as f:
    json.dump(dataset, f)
```