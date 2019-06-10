# Cppcheck 使用笔记

Cppcheck 是一个用于 C/C++ 代码静态分析的工具，可以比较好的检测出代码中潜在的 bug 和 未定义的行为。

未定义的行为有：

- Dead pointers
- Division by zero
- Integer overflows
- Invalid bit shift operands
- Invalid conversions
- Invalid usage of STL
- Memory management
- Null pointer dereferences
- Out of bounds checking
- Uninitialized variables
- Writing const data

日志及检查的类型有：

- **error** used when bugs are found
warning suggestions about defensive programming to prevent bugs
- **style** stylistic issues related to code cleanup (unused functions, redundant code, constness,
and such)
- **performance** Suggestions for making the code faster. These suggestions are only based on common
knowledge. It is not certain you'll get any measurable difference in speed by fixing
these messages.
- **portability** portability warnings. 64-bit portability. code might work different on different compilers. etc.
- **information** Configuration problems. The recommendation is to only enable these during configuration.

使用 `--enable` 限定检查类型输出：

``` text
# enable warning messages
cppcheck --enable=warning file.c
# enable performance messages
cppcheck --enable=performance file.c
# enable information messages
cppcheck --enable=information file.c
# For historical reasons, --enable=style enables warning, performance,
# portability and style messages. These are all reported as "style" when
# using the old xml format.
cppcheck --enable=style file.c
# enable warning and performance messages
cppcheck --enable=warning,performance file.c
# enable unusedFunction checking. This is not enabled by --enable=style
# because it doesn't work well on libraries.
cppcheck --enable=unusedFunction file.c
# enable all messages
cppcheck --enable=all
```

输出结果写入到文件：

``` text
cppcheck file1.c 2> err.txt
```

多线程检查：

``` text
cppcheck -j 4 path
```

导入项目的配置信息以实现预定义的宏的加入：

CMake 项目：

``` sh
cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=ON .
cppcheck --project=compile_commands.json
```

导入 xml 结果文件：

``` text
cppcheck --xml file1.cpp
```

最新版本：

``` text
1.84   2018-12-08
```

参考：

http://cppcheck.sourceforge.net/demo/
