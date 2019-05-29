## Caffe Python 接口使用中的问题记录

`make pytest` 时出现 numpy 某些模块缺失问题，原因是当前 python 环境中的 numpy 版本太高，不适配编译版本的 caffe

当前运行环境中的 numpy 版本可能为 `1.16.3`, 版本过高，推荐 `1.14.3`

```
pip install numpy==1.14.3
```
