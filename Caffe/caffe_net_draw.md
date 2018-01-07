# Caffe 神经网络模型绘制
两种方法
### 1.使用Netscope在线可视化工具
> 网址 [http://ethereon.github.io/netscope/quickstart.html](http://ethereon.github.io/netscope/quickstart.html)

把prototxt网络模型内容复制到选项框中，Shift+Enter即可
### 2. 使用Caffe提供的draw_net.py
1. 安装graphviz: `sudo apt install graphviz`
2. 安装pydot: `sudo pip install pydot`
3. 执行Caffe自带的draw_net.py
draw_net.py参数:
**[网络模型prototxt文件路径]**
**[保存图片的路径及图片名]**
**[--rankdir=x（可选）]**
 x可以是LR、RL、TB、BT，代表网络的方向，分别时从左到右，从右到左，从上到下，从下到上。默认为LR
> 例子：
```
 sudo python python/draw_net.py examples/mnist/lenet_train_test.prototxt netImage/lenet.png --rankdir=BT
```