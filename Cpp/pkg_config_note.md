# pkg-config 的一些使用笔记

`pkg-config` 搜索 `PKG_CONFIG_PATH`，再搜索默认路径 `/usr/lib/pkgconfig`

当在交叉编译时，不需要它搜索默认路径，以防止它链接到宿主机上的库

设置下面变量，指定到交叉工具链的 `sysroot/lib/pkgconfig`

``` sh
export PKG_CONFIG_LIBDIR=${sysroot}/lib/pkgconfig
```

摘录：

编译时，禁止pkg-config搜索默认目录 http://www.voidcn.com/article/p-sqzffoef-bec.html