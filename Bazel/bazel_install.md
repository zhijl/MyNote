# Bazel 从源码安装

基于Ubuntu 16.04，从源码安装Bazel

## 预安装

确保已安装或提前安装好JDK 8, Python, bash, zip, and the usual C build toolchain 

```
sudo apt-get install build-essential openjdk-8-jdk python zip
```

## 下载 Bazel源码

Github地址：https://github.com/bazelbuild/bazel/releases
需要选择dist版本，否则执行compile.sh的时候不成功，可能原因是没有配置好PROTOC，在编译源码的过程中需要使用到较新的protoc进行编译，dist版本自带protoc程序

## 编译 Bazel

```
bash compile.sh
```

编译成功后最后一行将显示

```
Build successful! Binary is here: /home/zhi/mydev/pre-install/output/bazel
```

## 参考
https://docs.bazel.build/versions/master/install-compile-source.html
