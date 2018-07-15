# Android 交叉编译动态库中遇到的问题

## problem 1

处理动态库或可执行程序中的 WARNING: linker

```
WARNING: linker: ./test: unused DT entry: type 0x6ffffffe arg 0x1b34
WARNING: linker: ./test: unused DT entry: type 0x6fffffff arg 0x3
```

solved ->
```
http://www.dllhook.com/post/211.html
https://github.com/kost/android-elf-cleaner
```

## problem 2

链接时出现未定义符 `atexit`

```
undefined reference to 'atexit'
```

solved ->
```
加上atexit的定义： extern "C" int atexit(void (*)()) {}, 这里atexit的原型要和stdlib.h中的一样
```

> 参考：https://blog.csdn.net/hp_truth/article/details/42459929

## problem 3

链接时出现未定义符 `__dso_handle`

```
undefined reference to '__dso_handle'
```

solved ->
```
extern "C"{ void * __dso_handle = 0 ;}  // to solve the ndk-build error
```

## problem 4

Android Studio 安装应用到手机上时出现 `INSTALL_FAILED_USER_RESTRICTED` 错误

solved ->

> 检查手机USB应用安装权限（一般在 **系统设置** 的 **开发者选项** 中设置）

## problem 5

Android 6.0 以上开发版本要求动态获取权限，如果编译的 APP 目标平台选为6.0及以上，可能会用一般的方法获取 IMEI 号码时会报权限问题

```
READ_PHONE_STATE
```

solved ->

> 比较简单的做法是将目标平台选为 6.0 以下，如 21