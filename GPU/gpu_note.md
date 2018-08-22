# GPU 方面的一些笔记

### 指定可见 GPU

通过设置环境变量 `CUDA_VISIBLE_DEVICES` 指定可见 GPU 设备

例如：

``` shell
# 如果在 /etc/profile 中添加
export CUDA_VISIBLE_DEVICES=0,1  # 仅显卡设备 0，1 可见，可通过 nvidia-smi 查看
```

或者在程序中指明（Python程序）：

``` python
import os
os.environ['CUDA_VISIBLE_DEVICES'] = 0,1
```
