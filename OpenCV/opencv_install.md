## OpenCV 3.3 编译安装
下面的步骤在Ubuntu 16.04 测试过有效，对于Ubuntu更高级版本适用问题不大
#### 所需的安装包
- GCC 5.4.及以上
- CMake 3.5及以上
- Git
- GTK+2及以上，包括头文件（libgtk2.0-dev）
- pkg-config
- Python2.7 Numpy
- ffmpeg, libav, development, libavcodec-dev, libavformat-dev, libswscale-dev
- libtbb2 libtbb-dev (可选)
- libdc1394 2.x （可选）
- libjpeg-dev, libpng-dev, libtiff-dev, libjasper-dev, libdc1394-22-dev （可选）
```
sudo apt install build-essential
sudo apt install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
sudo apt install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
```
#### 下载OpenCV源码
下载OpenCV 3.3源码包并解压 （https://github.com/opencv/opencv/releases）
#### CMake 执行编译
```
 cmake -D CMAKE_BUILD_TYPE=release -D CMAKE_INSTALL_PREFIX=/home/zhi/mydev/opencv ..
```
选项参数说明
- 设置编译类型:`CMAKE_BUILD_TYPE=Release\Debug`
- 设置编译来自opencv_contrib的模块: `set OPENCV_EXTRA_MODULES_PATH to <path to opencv_contrib/modules/>`
- 设置编译文档存放路径：`BUILD_DOCS`
- 设置编译样例：`BUILD_EXAMPLES`
[更多参考][1]
#### Make 执行编译
```
make -j7  # run 7 jobs in parallel，如果-j不加数字，则最大化并行数执行，一般-j适用于
```
#### Make 安装OpenCV
```
sudo make install
```

#### Reference：
[1]: http://blog.csdn.net/j_d_c/article/details/53365381 "CMake编译OpenCV各选项参数说明"

[1. OpenCV 3.1安装说明](https://docs.opencv.org/3.1.0/d7/d9f/tutorial_linux_install.html)

2017年11月23日20:34:37

2017年11月23日20:57:43