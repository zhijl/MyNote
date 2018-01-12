# LFW 人脸识别数据集

---

## LFW 人脸识别数据集简述

LFW，即 Labeled Faces in the Wiled,无约束自然场景人脸识别数据集，是一个因研究目的，为解决在非限制环境下的人脸识别问题而建立的数据集。该数据集包含了超过13,000多张全世界知名人士互联网自然场景不同朝向、表情和光照环境人脸图片组成，共有5749人。每张人脸图片都有其唯一的姓名ID和序号加以区分，其中有1680人有2张或2张以上人脸图片。

LFW数据集主要测试人脸识别的准确率，该数据库从中随机选择了6000对人脸组成了人脸辨识图片对，其中3000对属于同一个人2张人脸照片，3000对属于不同的人每人1张人脸照片。测试过程LFW给出一对照片，询问测试中的系统两张照片是不是同一个人，系统给出“是”或“否”的答案。通过6000对人脸测试结果的系统答案与真实答案的比值可以得到人脸识别准确率。

目前有四个不同的LFW图像数据集合，分别是包含了原始图像和其他三种不同对齐类型的图像。三种对齐图像分别是漏斗形图像（funneled image）（ICCV 2007），简称LFW-a，使用的是一种未发表的对齐方法。还有一种深度漏斗形图像（deep funneled）（NIPS 2012）。还有一种是由商业人脸对齐软件进行对齐的图像。

#### 小结：

* 13233 images
* 5749 people
* 1680 people with two or more images

数据集截图：

![lfw数据集截图](/_Resource/lfw_01.png)

lfw文件夹截图（每个子文件夹用名字命名）：

![lfw数据集截图](/_Resource/lfw_02.png)

## LFW 数据处理流程

LFW 数据集中的图片一般经过如下处理过程：

* 人脸检测(Viola-jones人脸检测器+人工矫正)

* 图片去重

* 标记人脸(保证名称的唯一性)

* 裁切与尺寸归一化(人脸框放大2.2倍，黑色填充，再resize到250×250)

* 组建训练测试集合

---

## LFW 数据集获取

1. 所有图片（tar压缩包）

http://vis-www.cs.umass.edu/lfw/lfw.tgz

2. 所有图片同时包含deep funneling对齐的图片

http://vis-www.cs.umass.edu/lfw/lfw-deepfunneled.tgz

3. 所有图片同时包含funneling对齐的图片

http://vis-www.cs.umass.edu/lfw/lfw-funneled.tgz

4. paris.txt 

http://vis-www.cs.umass.edu/lfw/pairs.txt

5. people.txt

http://vis-www.cs.umass.edu/lfw/people.txt

6. pairsDevTrain.txt

http://vis-www.cs.umass.edu/lfw/pairsDevTrain.txt

7. pairsDevTest.txt

http://vis-www.cs.umass.edu/lfw/pairsDevTest.txt

8. peopleDevTrain.txt

http://vis-www.cs.umass.edu/lfw/peoplesDevTrain.txt

9. peopleDevTest.txt

http://vis-www.cs.umass.edu/lfw/pairsDevTest.txt

---

## LFW 数据集使用说明

### 文件列表：

* lfw.tgz

* 列表文件

其中列表文件分别有：

总数据集列表文件：

* pairs.txt
* people.txt

训练测试数据集列表文件：

* pairsDevTrain.txt
* pairsDevTest.txt
* peopleDevTrain.txt
* peopleDevTest.txt

### 文件说明

#### 总图像文件 lfw.tgz

* 该压缩包包含LFW数据库的所有人脸图像，解压后，数据库的内容
将被放置在一个新的目录“lfw”中。
* 每个图像都可以用“lfw/name/name_xxxx.jpg”来表示，其中“xxxx”是图像编号前置填充为0四字符序号。例如，第十George_W_Bush图像命名为“lfw/George_W_Bush/George_W_Bush_0010.jpg”

* 数据库中共有13233个图像和5749个人

* 每幅图像都是大小为250x250的jpg格式图片，使用的是OpenCV中的Viola-Jones人脸检测器进行的人脸检测和居中。预先按缩放因子2.2进行放大，以便后面捕获更多头部，然后根据检测器返回的裁剪区域进行缩放，调整为一样的尺寸大小。

#### 训练模式

这里设定了两种视角的训练数据集

1. 图像给定模式

这种方式的训练数据仅限于pairs.txt文件中给出的图像对。

换句话说，如果一个匹配的对由George_W_Bush的第十和第十二张图像组成，另一个匹配对由George_W_Bush的第42和第50张图像组成，那么在这种模式下，将不允许交叉使用George_W_Bush匹配对中的图像组成新的匹配对，例如George_W_Bush的第10和第42个图像就是这种不被允许的情况。

为了确保这一点，将通过名字标识图像，但是不提供算法名字信息，也因此，这是被约束的，pairs.txt中的内容便可满足这种需求。

2. 图像随意模式

这种模式中，训练数据将以每个组中人员的名字和图像的形式一起被提供。这样，可以制定尽可能多的匹配以及不匹配成对。

例如，如果George_W_Bush和John_Kerry都出现在一组中，
那么任何一对George_W_Bush的图像都可以用作匹配对，并且
可以将任何一张George_W_Bush的图像与John_Kerry的任何一张图像进行组成
以形成不匹配对。

这种方式是图像随意的，提供给每个组中人员名字即可，在people.txt文件中有说明。

3. 测试程序

在两种模式下，测试过程是相同的。也就是说，训练集是由10组中的9组构成的（十折交叉验证），其中测试集由剩下的一组。然后算法必须对剩下的这组中的每一对图像进行分类。换句话说，算法每次仅能针对一对图像进行分类，而不能涉及到其他测试对。

需注意的是，为了计算方便比较测试性能，需要统一使用pairs.txt中的图像对，即使在图像随意模式下也是如此。另外，在图像随意模式下，可以从不同组中交叉形成不匹配对训练图像。

4. 训练、验证和测试

我们将数据集组织为两个“视图”。视图1用于在正式评估之前，算法开发和一般实验，即模型选择或验证。视图2用于性能测试，即方法的最终评估。

* 4.1 角度1：针对于算法的开发使用

在这种情况下两种模式下的数据集（图像给定和图像随意）一致。第一种模式由pairsDevTraining.txt和pairsDevTest.txt组成。这两个文件内容的格式是：

第一行给出匹配对的数目N（等于不匹配对的数目），接着是N行匹配对和N行不匹配对，格式与pairs.txt文件中的匹配对相同。

第二种模式由peopleDevTraining.txt和peopleDevTest.txt两个文件组成。这两个文件的内容格式是：第一行给出人数N，之后的N行时同pairs.txt中一样的匹配对。

* 4.2 角度2：作为性能测试的标准使用

随机地将数据集分为10份，然后在每份子数据集中随机地选择300个匹配对和300个不匹配对。匹配对的信息在pairs.txt中提供。为了实现这种划分模式，可以使用10折交叉验证的方式。

* 4.3 pairs.txt 内容格式说明

pairs.txt文件的内容格式如下：第一行给出匹配对的数量N，不匹配对的数量也等于匹配对的数量，也为N。这里的N实际为300，一共有10组，每一个由300个匹配对和300个不匹配对组成。例如，后面行紧接着是第一组数据子集的300个匹配对，每行为一个匹配对，格式如下：

name    n1  n2

这里的匹配对由name这个人不同的两张图像n1和n2组成，例如

George_W_Bush   10    24

意味着匹配对指的是George_W_Bush_0010.jpg和George_W_Bush_0024.jpg这两张图片。

再后面的300行为不匹配对，格式说明如下：

name1   n1  name2   n2

这意味着不匹配对由name1这个人的第n1张图像和name2这个人的第n2个图像组成。例如：

George_W_Bush   12  John_Kerry  8

这里的不匹配对指的时George_W_Bush_0012.jpg和John_Kerry_0008这两张图片。

后面将的9组格式同上。在pairs.txt总共时3000个匹配对和3000个不匹配对。

* 4.4 people.txt内容格式说明
 
people.txt文件格式如下：第一行给出子数据集的个数（因为随机划分为了10个数据集，所以个数为10）。后面一行给出第一个子数据集中的人数N。接下来的N行给出的名字和每个人的人脸图像数量。每一行为一个人的信息，例如：

George_W_Bush 530 

后面依次重复9次，表示其他9组的数据信息。

---

## 结果评测指标

### 准确率

准确率即正确分类正确的次数占总分类次数的比率。

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

---

## 总结和评价

LFW是针对早期人脸验证(Face Verification)任务提出评测方法与指标，结果有借鉴意义，但已不代表目前的最难问题。

简单地说LFW就是用来测试这些小孩(算法)够不够上幼儿园的智力水平测试。为什么说它是题库，就是因为这6000组网络样本——6000张照片，是固定的。任何一个系统都可以对这6000组样本进行有针对性的优化，从而达到刷高分的效果。


## 参考文献

[LFW 主页](http://vis-www.cs.umass.edu/lfw/)

[FDDB和LFW数据集浅析及刷分心得](https://zhuanlan.zhihu.com/p/23886731?utm_source=tuicool&utm_medium=referral)

[LFW pairs.txt解释](http://blog.csdn.net/zhongzhongzhen/article/details/78293789)