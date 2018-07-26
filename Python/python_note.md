# 记录 Python 编程中的一些笔记

### csv 文件写入空白行

数据逐行写入 csv 文件时，每条数据后面会有一行空白行，解决方法

python2：`open` 文件时操作模式加上 `b`
python3: `open` 参数中打开 `newline=''`

参考：

https://blog.csdn.net/akenseren/article/details/79364356
