# 记录 Python 编程中的一些笔记

### lambda 用法

lambda 函数也叫匿名函数，及即没有具体名称的函数，它允许快速定义单行函数，类似于C语言的宏，可以用在任何需要函数的地方

lambda 与 def 的区别：

- def 创建的方法是有名称的，而 lambda 没有
- lambda 会返回一个函数对象，但这个对象不会赋给一个标识符，而def则会把函数对象赋值给一个变量（函数名）
- lambda 只是一个表达式，而def则是一个语句
- lambda 表达式 ":" 后面，只能有一个表达式，def则可以有多个
- 像 if 或 for 或 print 等语句不能用于lambda中，def可以
- lambda 一般用来定义简单的函数，而 def可以定义复杂的函数
- lambda 函数不能共享给别的程序调用，def 可以

lambda 语法格式：

``` text
lambda 参数1, 参数2, ... : 要执行的语句
```

### csv 文件写入空白行

数据逐行写入 csv 文件时，每条数据后面会有一行空白行，解决方法

python2：`open` 文件时操作模式加上 `b`
python3: `open` 参数中打开 `newline=''`

参考：

https://blog.csdn.net/akenseren/article/details/79364356

https://blog.csdn.net/SeeTheWorld518/article/details/46959593