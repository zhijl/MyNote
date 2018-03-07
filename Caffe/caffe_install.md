## Caffe 安装指南

下面安装过程中 `prefix` 为安装路径

### 下载源码
从GitHub上获取源码，可以使用Git或者wget
```
wget https://github.com/BVLC/caffe/archive/1.0.tar.gz
```

### 依赖程序安装
- BLAS 
- Boost
- protobuf
- glog
- gflags
- hdf5

可选
- CUDA
- cuDNN
- OpenCV
- IO库：lmdb或leveldb（leveldb需要snapy）

### Protobuf 安装和配置

1. 下载Protobuf源码

https://github.com/google/protobuf/releases

2. 解压和编译

```
> ./configure --prefix=/home/zhi/mydev
> make
> make install
```

### Boost 安装和配置

1. 下载Boost源码

https://dl.bintray.com/boostorg/release/1.66.0/source/

2. 解压和编译

```
> tar zxvf boost_1_66_0_rc2.tar.gz
> cd boost_1_66_0/
> ./bootstrap.sh --with-libraries=system,filesystem,thread,python
> ./b2

```

```
> cp -r boost/ /mydev/include/
> cp stage/lib/* /mydev/lib/
```

### GFLAGS 安装和配置

1. 下载和解压源码
https://github.com/gflags/gflags/releases

2. 编译
```
> mkdir build
> cmake ..
> ccmake ..
将BUILD-SHARED_LIBS修改为ON（回车）
将CMAKE_INSTALL_PREFIX修改为自己的目录地址
按c保存配置
> make
> make install
```
### GLOG 安装和配置

1. 下载和解压源码

https://github.com/google/glog

2. 编译

```
> ./configure --prefix=/home/zhi/mydev
> make
> make install
```

### OpenBLAS 安装和配置

1. 下载和解压
https://github.com/xianyi/OpenBLAS/archive/v0.2.20.tar.gz

2. 编译

```
> make -j
> make PREFIX=/home/zhi/mydev install
```

### HDF5 安装和配置

1. 下载和解压

https://github.com/live-clones/hdf5/archive/hdf5-1_8_20.tar.gz

2. 安装和配置

```
> ./configure --prefix=/home/zhi/mydev
> make -j
> make install
```

### LEVELDB 安装和配置

1. 下载和解压

https://github.com/google/leveldb/releases

2. 安装和配置

```
> make
> cp -r include/leveldb ~/mydev/include
> cp out-shared/libleveldb.so* ~/mydev/lib
```

### Snappy 安装和配置

1. 下载和解压

https://github.com/google/leveldb/releases

2. 安装和配置

```
> ./configure --prefix=/home/zhi/mydev
> make
> make install
```

### 配置环境变量

将`export PATH=~/mydev/bin/:$PATH`加入到`~/.bashrc`中

``` shell
> source ~/.bashrc
# 让修改生效
```

### 根据需求修改 Makefile.config 文件

修改完成后，进行编译

```
make -j
make distribute
```

### 仍然遇到的问题

``` shell
> make pycaffe
python/caffe/_caffe.cpp:1:52: fatal error: Python.h: No such file or directory
```

解决——注意配置文件中的anaconda应该为anaconda2

``` shell
 72 ANACONDA_HOME := $(HOME)/anaconda2
 73 PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
 74         $(ANACONDA_HOME)/include/python2.7 \
 75         $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include
```

OpenBlas的问题:

``` shell
CXX/LD -o .build_release/tools/upgrade_solver_proto_text.bin
.build_release/lib/libcaffe.so: undefined reference to `cblas_ddot'
.build_release/lib/libcaffe.so: undefined reference to `cblas_daxpy'
.build_release/lib/libcaffe.so: undefined reference to `cblas_dasum'
.build_release/lib/libcaffe.so: undefined reference to `cblas_dcopy'
.build_release/lib/libcaffe.so: undefined reference to `cblas_sdot'
.build_release/lib/libcaffe.so: undefined reference to `cblas_dscal'
.build_release/lib/libcaffe.so: undefined reference to `cblas_sgemm'
.build_release/lib/libcaffe.so: undefined reference to `cblas_dgemm'
.build_release/lib/libcaffe.so: undefined reference to `cblas_sgemv'
.build_release/lib/libcaffe.so: undefined reference to `cblas_sscal'
.build_release/lib/libcaffe.so: undefined reference to `cblas_scopy'
.build_release/lib/libcaffe.so: undefined reference to `cblas_saxpy'
.build_release/lib/libcaffe.so: undefined reference to `cblas_dgemv'
.build_release/lib/libcaffe.so: undefined reference to `cblas_sasum'
collect2: error: ld returned 1 exit status
Makefile:625: recipe for target '.build_release/tools/upgrade_solver_proto_text.bin' failed
make: *** [.build_release/tools/upgrade_solver_proto_text.bin] Error 1
```

解决——手动在 Makefile 中的 `LDFLAGS +=` 处加上 `-lopenblas`

启用OpenBlas加速

``` shell
export OPENBLAS_NUM_THREADS=4
```

测试

``` shell
export LD_LIBRARY_PATH=/home/zhi/mydev/lib:$LD_LIBRARY_PATH
make test -j4
make runtest
```

2017年11月23日21:16:35
2017年12月26日22:32:20 update
