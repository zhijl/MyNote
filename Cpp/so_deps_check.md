# 检查动态库缺失符号问题

Linux 下编译的动态库正常生成后，在用的过程中可能才会暴露出依赖的缺失问题，所以有必要通过一些工具提前进行检查

C/C++ 的编译器中提供了这样一些工具 `ldd`、`nm`、`c++filt`，可以帮助我们完成这样的检查

### 查找未定义的符号

可以使用 `ldd` 和 `nm` 工具找出 so 库中未定义的符号名

``` sh
ldd -r xxx.so
```

出现类似 `_ZN12FaceDetectorC1Ev` 这样的符号说明含有未定义的符号，也即有依赖缺失

可以进一步通过 `c++filt` 工具解码出符号对应的函数名

``` sh
c++filt _ZN12FaceDetectorC1Ev
```

原符号名为 `FaceDetector::FaceDetector()`

通过这样的检查我们就能很方便地定位到是具体是哪个库没链接进来导致的依赖缺失

### C/C++ 编译器的命名粉碎规则（name mangling）

编译器会把所有的方法放到**虚拟方法表**中，为了对每一个声明（类、函数、全局变量等）进行索引，设计了一套字符串格式，用来映射代码中的类、函数或者全局变量等声明