# CMake 零碎笔记

### Ubuntu 上安装 CMake 和 CCMake

``` shell
sudo apt-get install cmake-curses-gui
```

### 在 CMake 中 Strip 目标文件

对目标程序、动态库进行 strip

一种不太友好的做法是：

``` cmake
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -s")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -s")
```

推荐的配置方法：

``` cmake
set_target_properties(TARGET_NAME PROPERTIES LINK_FLAGS_RELEASE -s)
```

### 在 CMake 中静态链接 glibc

target_link_libraries(MYLIB -static-libgcc -static-libstdc++)

### 参考

https://my.oschina.net/iamhere/blog/515660
https://www.cnblogs.com/rickyk/p/3884257.html
https://www.technovelty.org/linux/exploring-origin.html
http://blog.csdn.net/dreamcs/article/details/52138229
https://www.cnblogs.com/autophyte/p/6147751.html
https://stackoverflow.com/questions/38675403/how-to-config-cmake-for-strip-file
