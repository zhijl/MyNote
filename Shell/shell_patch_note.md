
## 快速生成某一目录下所有文件的 md5 值

``` shell
find FIT_JNI_libs_armv7/ -type f -print0 | xargs -0 md5sum > FIT_JNI_libs_armv7/fit_jni_libs_armv7.md5
```

## 输出一行变为一列

``` shell
cat inputfile | tr "\n" " "
```