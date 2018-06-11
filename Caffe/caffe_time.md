# Caffe 测试网络模型时间

```
caffe time -model=shufflenet_deploy.prototxt -iterations=100         # cpu
caffe time -model=shufflenet_deploy.prototxt -iterations=100 -gpu=0
```
