# 编译过程中的一些知识点记录

## C/C++ 库

库的目的是代码重用，提供共用的功能，某个程序为别的程序提供公开的接口等

### 共享库的搜索顺序

共享库搜索顺序一般是 $LDLIBRARY_PATH，/etc/ld.so.cache, /usr/lib, /lib

### 命名和编号

- 所有库名以 lib 开头，gcc 在 -l 指定的文件名前自动插入 lib，如 libmysqlclient 就用 -lmysqlclient
- .a 是静态库(archive)，如 libmysqlclient.a
- .so 是共享库(shared object)，如 libmysqlclient.so
- 编号格式：library_name.major_num.minor_num.patch_num，如 libmysqlclient.so.15.0.0
- _g 和 _p: /usr/lib/libform_g.a 中的 _g 表示这是 libform.a 的调试库，用 `locate xx_g.a` 会发现很多类似的库，但用 `locate xx_g.so` 没有发现 FC4 有此类库；libxxx_p.a 中的 _p 表示这是 libxxx.a 的性能分析库（profiling），但用 `locate xx_p.a` 和 `locate xx_p.so` 没有发现 FC4 有此类库（？）

库要和接口头文件配合使用，常见的库如：

- libc.so (不需头文件) 标准C库
- libdl.so (dlfcn.h) 让程序在运行是加载和使用库代码，而不在编译时链接库
- libglib.so (glib.h) Glib工具函数，例如hash, string等
- libgthread.so (glib.h) 对Glib的线程支持
- libm.so (math.h) 标准C数学库
- libpthread.so (pthread.h) POSIX标准Linux线程库
- libz.so (zlib.h) 通用压缩程序库

### 库操作命令

- nm 列出目标文件或二进制文件的所有符号
- ar 创建静态库和符号索引
- ldd 列出程序正常运行所需要的共享库，例如

``` text
linux-gate.so.1 => (0x00c59000)
libmysqlclient.so.15 => /lib/libmysqlclient.so.15 (0x009a1000)
libc.so.6 => /lib/libc.so.6 (0x0038b000)
libpthread.so.0 => /lib/libpthread.so.0 (0x004f8000)
libcrypt.so.1 => /lib/libcrypt.so.1 (0x002f0000)
libnsl.so.1 => /lib/libnsl.so.1 (0x00320000)
libm.so.6 => /lib/libm.so.6 (0x004bd000)
/lib/ld-linux.so.2 (0x0036d000)
```

- ldconfig 和动态链接和装载工具 ld.so/ld-linux.so 一起决定位于 /usr/lib 和 /lib 下的 so 库所需的链接。ldconfig 创建一个从实际库到so库名的符号链接。注意 /etc/ld.so.cache, /etc/ld.so.conf `ldconfig -p` 列出 /etc/ld.so.cache 内的库对照链接

### 相关环境变量

动态链接器 ld.so/ld-linux.so 使用一些环境变量：

`$LDLIBRARY_PATH`：格式类似 $PATH，:分隔，非标准位置 /usr/lib 和 /lib 下的库或者 /etc/ld.so.cache 中没有的库，需要加入该变量才能被搜索到

`$LD_PRELOAD`：空格分隔，定义需要在最前面加载的库。也可以由 /etc/ld.so.preload 文件代替

静态库和共享库都是包含 object 文件的文件

### 建立和使用静态库

1. 把代码编译成目标文件，如 gcc -c libxxx.c -o libxxx.o
2. ar: ar -rcs linxxx.a linxxx.o
3. gcc -static: gcc test.c -o test -static -L. -lxxx
4. 用 file 检查静态链接的可执行文件
5. 用nm检查符号，静态链接没有未定义符号

共享库占用系统资源少（磁盘和内存），运行时根据共享链接从单个文件加载，速度快，维护方便。在运行时，ld.so/ld-linux.so 把二进制文件中的符号名链接到适当的 so 库上

### 建立和使用共享库

1. gcc -fPIC 产生与位置无关的代码，如 gcc -fPIC -g -c libxxx.c -o libxxx.o
2. gcc -shared 和 -soname，如 gcc -g -shared -Wl,–soname, -libxxx.so -o libxxx.so.1.0.0 libxxx.o (注意 -Wl,–soname, -libxxx.so 中间没有空格)
3. gcc -Wl 把参数传递给链接器 ld
4. gcc -l 显式链接 C 库

转自：
https://blog.csdn.net/sun_x_t/article/details/7171771