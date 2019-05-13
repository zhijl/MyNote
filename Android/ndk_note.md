# 记录使用 NDK 的一些笔记

### `libc++_shared.so not found` 问题

有一次在使用高版本的 NDK （r18）编译一个测试程序时，运行的时候提示没有 `libc++_shared.so` 库，这个库应该是一个标准库，我以为把 NDK 目录中的 `libc++_shared.so` 拷贝到 `/system/lib` 下就可以了，但是拷不过去。

### 解决 ->

编译时链接静态库 `libc++_shared.a`，即编译参数改为 `-static-libstdc++`

https://developer.android.com/ndk/guides/cpp-support#libc
