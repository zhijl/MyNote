# Protocol Linux 平台安装

## 步骤

1. 下载protocol

> [https://github.com/google/protobuf/tags](https://github.com/google/protobuf/tags)

1. 配置和安装

解压压缩包，进入目录，执行

``` shell
./configure --prefix=$INSTALL_DIR
make
make check
sudo make install
```

**CMake 方式编译**

``` sh
git clone https://github.com/protocolbuffers/protobuf.git
cd protobuf/cmake
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=install -Dprotobuf_BUILD_TESTS=NO -DBUILD_SHARED_LIBS=OFF ..
make
make install
```

**INSTALL_DIR**  自定义安装目录

如果要使用Protocol的Python接口，在解压目录下还需要将python目录中，执行

``` shell
python setup.py
```

生成其他文件

> 2017年11月24日18:23:46  zhijl
