# Theano 使用过程中遇到的问题

### Theano 的 GPU 使用

配置文件的方式配置：

```
vim ~/.theanorc
```

调加：

``` text
[global]
floatX=float32
device=cuda{2}
```

上面表示使用 GPU 设备ID为 2 的GPU，多 GPU 即 {0,1,2,3}

遇到的问题有：

``` text
ERROR (theano.gpuarray): pygpu was configured but could not be imported or is too old (version 0.6 or higher required)
```

``` text
ERROR (theano.gpuarray): Could not initialize pygpu, support disabled
Traceback (most recent call last):
  File "/home/rd5/anaconda3/envs/pytorch3.1/lib/python3.6/site-packages/theano/gpuarray/__init__.py", line 164, in <module>
    use(config.device)
  File "/home/rd5/anaconda3/envs/pytorch3.1/lib/python3.6/site-packages/theano/gpuarray/__init__.py", line 151, in use
    init_dev(device)
  File "/home/rd5/anaconda3/envs/pytorch3.1/lib/python3.6/site-packages/theano/gpuarray/__init__.py", line 60, in init_dev
    sched=config.gpuarray.sched)
  File "pygpu/gpuarray.pyx", line 634, in pygpu.gpuarray.init
  File "pygpu/gpuarray.pyx", line 584, in pygpu.gpuarray.pygpu_init
  File "pygpu/gpuarray.pyx", line 1057, in pygpu.gpuarray.GpuContext.__cinit__
pygpu.gpuarray.GpuArrayException: b'cuDeviceGet: CUDA_ERROR_INVALID_DEVICE: invalid device ordinal'
```

``` text
ERROR (theano.gpuarray): Could not initialize pygpu, support disabled
Traceback (most recent call last):
  File "/home/rd5/anaconda3/envs/pytorch3.1/lib/python3.6/site-packages/theano/gpuarray/__init__.py", line 164, in <module>
    use(config.device)
  File "/home/rd5/anaconda3/envs/pytorch3.1/lib/python3.6/site-packages/theano/gpuarray/__init__.py", line 151, in use
    init_dev(device)
  File "/home/rd5/anaconda3/envs/pytorch3.1/lib/python3.6/site-packages/theano/gpuarray/__init__.py", line 60, in init_dev
    sched=config.gpuarray.sched)
  File "pygpu/gpuarray.pyx", line 634, in pygpu.gpuarray.init
  File "pygpu/gpuarray.pyx", line 572, in pygpu.gpuarray.pygpu_init
ValueError: invalid literal for int() with base 10: '{2}'
```
