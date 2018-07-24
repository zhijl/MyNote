# 卷积神经网络中的一些计算

``` text
# 计算输出图大小
W: image width
H: image height
S: stride
P: padding
K: kernel

W': output feature map width
H': output feature map height

W' = (W-K+2P)/S + 1
H' = (H-K+2P)/S + 1

# 网络计算量
K: kernel size
M: feature map size
CI: input channel number
CO: output channel number
g: group number

Pool:
[M*M*K*K*CI]

Normal Conv
[M*M*CI*K*K*CO]

Depthwise Conv
[M*M*CI*K*K + M*M*CI*CO]

Group Conv
[M*M*(CI/g)*K*K*(CO/g)*g]
```