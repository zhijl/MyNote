## 在Ubuntu平台安装 HDF5
### 下载和解压源码
GitHub下载地址：(https://github.com/live-clones/hdf5/releases)[https://github.com/live-clones/hdf5/releases]
```
wget https://github.com/live-clones/hdf5/archive/hdf5-1_8_19.tar.gz
tar zxvf hdf5-1_8_19.tar.gz
```
### 配置
加入程序安放路径
```
./configure --prefix=/home/zhijinlin/mydev/
```

### 编译和安装
```
make -j
make install
```


2017年11月24日11:51:48

