#### gcc 混合链接动态库和静态库

当对动态库与静态库混合连接的时候，使用-static会导致所有的库都使用静态连接的方式。这时需要作用`-Wl`的方式：

``` shell
gcc test.cpp -L. -Wl,-Bstatic -ltestlib  -Wl,-Bdynamic -ltestlib
```

注意`-Wl`会将选项参数转传给ld链接器

#### CMake 中加入 fPIC

有时链接时由于平台原因会导致 `relocation R_X86_64_32 against 'xxx' can not be used when making a shared object; recompile with -fPIC`

提示某些库需要加上 `-fPIC` 参数重新编译

直接在 `CMakeLists.txt` 前面加入

``` cmake
target_compile_options(myLib PRIVATE -fPIC)

add_compile_options(-fPIC)  # 通常直接在前面加这一句就会有效！

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fpic")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fpic")
```

或者

``` cmake
set(CMAKE_POSITION_INDEPENDENT_CODE ON)  # 通常直接在前面加这一句也会有效！

add_library(lib1 SHARED lib1.cpp)
set_property(TARGET lib1 PROPERTY POSITION_INDEPENDENT_CODE ON)
```

#### 判断一个链接库是否是 fPIC

`fPIC` 的目的是什么？共享对象可能会被不同的进程加载到不同的位置上，如果共享对象中的指令使用了绝对地址、外部模块地址，那么在共享对象被加载时就必须根据相关模块的加载位置对这个地址做调整，也就是修改这些地址，让它在对应进程中能正确访问，而被修改到的段就不能实现多进程共享一份物理内存，它们在每个进程中都必须有一份物理内存的拷贝。`fPIC` 指令就是为了让使用到同一个共享对象的多个进程能尽可能多的共享物理内存，它背后把那些涉及到绝对地址、外部模块地址访问的地方都抽离出来，保证代码段的内容可以多进程相同，实现共享

`-fPIC` 作用于编译阶段，告诉编译器产生与位置无关代码(Position-Independent Code)，则产生的代码中，没有绝对地址，全部使用相对地址，故而代码可以被加载器加载到内存的任意位置，都可以正确的执行。这正是共享库所要求的，共享库被加载时，在内存的位置不是固定的

Linux下编译共享库时，必须加上 `-fPIC` 参数

但是如果有如下需求，可以不考虑加 `-fPIC`

1. 该库可能需要经常更新
1. 该库需要非常高的效率(尤其是有很多全局量的使用时)
1. 该库并不很大
1. 该库基本不需要被多个应用程序共享

检查某个动态库是否 `fPIC`

``` shell
readelf -d foo.so |grep TEXTRE
```

如果上边的 shell 有任何输出，则说明这 `foo.so` 不是 `PIC`，`TEXTREL` 表示代码段重定位表地址，`PIC` 的共享对象不会包含任何代码段重定位表

### 参考

1. [gcc 混合连接动态库和静态库](http://blog.csdn.net/sunlion81/article/details/9963963)
1. [Linux共享对象之编译参数fPIC](https://www.cnblogs.com/cswuyg/p/3830703.html)