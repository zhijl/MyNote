# tar 加密压缩/解压

- 压缩

``` shell
tar -czvf /path/to/file.tar.gz file
```

- 解压

``` shell
tar -xzvf /path/to/file.tar.gz /path/to
```

- 加密压缩

``` shell
tar -zcvf - file | openssl des3 -salt -k input_password -out /path/to/file.tar.gz
```

- 加密解压

``` shell
openssl des3 -d -k input_password -salt -in /path/to/file.tar.gz | tar zxvf -
```

### 参考

1. [tar压缩/解压、加密压缩/解密解压](https://blog.csdn.net/yanglishuan/article/details/47687139)
