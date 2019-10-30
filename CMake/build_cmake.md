# 编译高版本的 cmake

cmake 也可以通过 conda 直接从源里安装，但是安装的程序不包含 ccmake

### 安装步骤

- 系统平台: Ubuntu 16.04
- 从源中安装预编译的 cmake 和 ccmake: `sudo apt install cmake cmake-curses-gui`，该 cmake 为 3.5.1 版本
- 下载 cmake 新版本源代码，如 3.13.2 版本
- 编译 cmake: ./configure --prefix=/usr/local/cmake-3.13.2
- 修改配置 `BUILD_CursesDialog` 为 `ON`，可通过 ccmake 进行修改
- `make & make install` 即可 