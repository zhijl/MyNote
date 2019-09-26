# CMake 交叉编译配置

摘录的一些有用的笔记

引用：

1. [CMake交叉编译配置](https://www.cnblogs.com/rickyk/p/3875334.html)
2. [CMake交叉编译](http://zhixinliu.com/2016/02/01/2016-02-01-cmake-cross-compile/)

### 搜索查找外部依赖

稍微大一点的项目都会用到一些外部依赖库或者 tool，CMake 提供了 `FIND_PROGRAM()`, `FIND_LIBRARY()`, `FIND_FILE()`, `FIND_PATH()` and `FIND_PACKAGE()` 等命令来进行外部依赖的搜索查找

但是有个问题，假如我们在给一个 ARM 处理器的移动设备做交叉编译，其中需要寻找 `libjpeg.so`，假如F `IND_PACKAGE(JPEG)` 返回的是 `/usr/lib/libjpeg.so`，那么这就会有问题，因为找到的这个 so 库只是给你的宿主机系统(例如一个 x86 的 Ubuntu 主机)服务的，不能用于 ARM 系统。所以你需要告诉 CMake 去其它地方去查找，这个时候你就咬配置以下的变量了：

- `CMAKE_FIND_ROOT_PATH`:

代表了一系列的相关文件夹路径的根路径的变更，比如你设置了 `/opt/arm/`，所有的 `Find_xxx.cmake` 都会优先根据这个路径下的 `/usr/lib`、`/lib` 等进行查找，然后才会去你自己的 `/usr/lib` 和 `/lib` 进行查找

- `CMAKE_FIND_ROOT_PATH_MODE_PROGRAM`:

对 `FIND_PROGRAM()` 起作用，有三种取值，`NEVER`、`ONLY`、`BOTH`，第一个表示不在你 `CMAKE_FIND_ROOT_PATH` 设置的目录下进行查找，第二个表示只在这个路径下查找，第三个表示先查找这个路径，再查找全局路径

- `CMAKE_FIND_ROOT_PATH_MODE_LIBRARY`

对 `FIND_LIBRARY()` 起作用，表示在链接的时候的库的相关选项，因此这里需要设置成ONLY来保证我们的库是在交叉环境中找的

#### 一个小例子

附上一个 `CMake` 官方文档中的 `toolchian file` 的小例子，这样我们就会对如何写 `toolchain` 文件有了感性认识：

``` cmake
# this one is important
SET(CMAKE_SYSTEM_NAME Linux)
#this one not so much
SET(CMAKE_SYSTEM_VERSION 1)

# specify the cross compiler
SET(CMAKE_C_COMPILER   /opt/eldk-2007-01-19/usr/bin/ppc_74xx-gcc)
SET(CMAKE_CXX_COMPILER /opt/eldk-2007-01-19/usr/bin/ppc_74xx-g++)

# where is the target environment 
SET(CMAKE_FIND_ROOT_PATH  /opt/eldk-2007-01-19/ppc_74xx /home/alex/eldk-ppc74xx-inst)

# search for programs in the build host directories
SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
# for libraries and headers in the target directories
SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
```

执行起来也很简单，如下：

``` sh
~/src$ cd build
~/src/build$ cmake -DCMAKE_TOOLCHAIN_FILE=~/Toolchain-eldk-ppc74xx.cmake ..
```
