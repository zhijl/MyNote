# FDDB 人脸检测数据集

---

## FDDB 人脸检测数据集简述

FDDB，Face Detection Data Set and Benchmark，是全世界最具权威的人脸检测评测平台之一，包含2845张图片，共有5171个人脸作为测试集。测试集范围包括：不同姿势、不同分辨率、旋转和遮挡等图片，同时包括灰度图和彩色图，标准的人脸标注区域为椭圆形。

图片来源：美联社和路透社新闻报道图片，并删除了重复图片。

![FDDB数据集图片](/_Resource/fddb_01.jpg)
![FDDB数据集图片](/_Resource/fddb_02.jpg)

---

## 数据集获取

* 原始图像集

http://tamaraberg.com/faceDataset/originalPics.tar.gz

* 人脸标注信息

http://vis-www.cs.umass.edu/fddb/FDDB-folds.tgz

---

## 数据集使用说明

文件列表：

- 原始图像集

- 人脸标注文件

检测输出方式：

- 矩形区域标注文件
- 椭圆形区域标注文件

- 其他信息

### 原始图像数据

原始图像数据集集中存放于originalPics.tar.gz压缩包中，解压originalPics.tar.gz，将生成一个originalPics文件夹，每一张原始图像按照 originalPics/year/month/day/big/*.jpg 的形式进行存放

### 人脸标注信息

解压FDDB-folds.tgz，将生成一个FDDB-folds文件夹，里面有FDDB-fold-xx.txt和FDDB-fold-xx-ellipseList.txt系列文件，xx = {01, 02, ..., 10} 索引每个子文件夹。

FDDB-fold-xx.txt文件中的每一行分别记录图片在原始图像集中的图片路径。例如

```
2002/07/19/big/img_130
```

标识的是原始图像数据集中的2002/07/19/big/img_130.jpg文件

标注信息存放在每个FDDB-fold-xx-ellipseList.txt文件中，文件内容的格式如下：

```
...
<image name i>   # 图片名称
<number of faces in this image =im>  # 这张图片中的人脸个数
<face i1>  # 第一个人脸的标注信息
<face i2>  # 第二个人脸的标注信息
...
<face im>
...
```

这里，为每张图片标注，脸部椭圆标注信息为

```
<major_axis_radius minor_axis_radius angle center_x center_y detection_score>
```

分别表示：
major_axis_radius —— 长半轴
minor_axis_radius —— 短半轴
angle —— 偏移角度
center_x —— 中心点x坐标
center_y —— 中心点y坐标
detection_score —— 检测分数

脸部矩形标注信息为：

```
<left_x top_y width height detection_score> 
```

分别表示：
left_x —— 矩形左上角坐标x
top_y —— 矩形左上交坐标y
width —— 矩形宽
height —— 矩形高
detection_score —— 检测分数

---

## 结果评判标准

结果的评判从两个方面来看，一个是召回率，另一个是ROC曲线。

### 匹配度

匹配度是判断检测区域与标注区域的匹配程度，匹配度大于等于0.5说明检测区域为人脸区域，属于正确检测；匹配度小于0.5说明非人脸区域，属于误报。

匹配度计算公式如下：

$$
S(d_i, l_j) = \dfrac{area(d_i) \cap area(l_i)} {area(l_i)}
$$

这里$area(d_i)$表示检测第i张被检测到的人脸区域，$area(l_i)$表示第i张对应的人脸标注区域。

### 召回率

召回率，表示样本中的正例有多少被预测正确了。那也有两种可能，一种是把原来的正类预测成正类($TP$)，另一种就是把原来的正类预测为负类($FN$)。

召回率的计算公式如下：

$$
P=\dfrac{TP}{TP+FP}
$$

### ROC曲线

ROC，即 receiver operating characteristic，在逻辑回归里面，我们会设一个阈值，大于这个值的为正类，小于这个值为负类。如果我们减小这个阀值，那么更多的样本会被识别为正类。这会提高正类的识别率，但同时也会使得更多的负类被错误识别为正类。为了形象化这一变化，在此引入 ROC ，ROC 曲线可以用于评价一个分类器好坏。

ROC关注两个指标，

$$
TPR=\dfrac{TP}{TP+FN}
$$

$$
FPR=\dfrac{FP}{FP+TN} 
$$

$TPR$ 代表能将正例分对的概率，$FPR$ 代表将负例错分为正例的概率。在 $ROC$ 空间中，每个点的横坐标是 $FPR$，纵坐标是 $TPR$，这也就描绘了分类器在 $TP$（真正率）和 $FP$（假正率）间的权衡。

ROC曲线图：

![ROC曲线图](/_Resource/roc_01.png)

---

## 参考

[FDDB人脸检测测评数据集介绍](http://blog.csdn.net/xzzppp/article/details/51779359)

[Windows下如何在FDDB数据库上评测自己的人脸检测分类器](http://blog.csdn.net/mr_curry/article/details/52141730)


[FDDB](https://charlesnord.github.io/2017/04/07/FDDB/)

[机器学习性能评估指标（精确率、召回率、ROC、AUC）](http://blog.csdn.net/u012089317/article/details/52156514)
