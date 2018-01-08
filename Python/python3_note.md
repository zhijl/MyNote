# Python3 零碎笔记

## 输入输出
* `print` 函数

1. 可以接受多个字符串，用逗号“,”隔开，就可以连成一串输出
2. `print`会依次打印每个字符串，遇到逗号“,”会输出一个空格
3. `print`也可以打印整数，或者计算结果

* 例子

```
> print('abc', 'def', 'hij')

abc def hij

> print('10 + 20 =', 10 + 20)

30
```

* `input` 函数

> 从终端读入字符串，回车停止读入

* 将四个空格做为缩进标准

* `r''`引号内部的字符串不进行转移

例子：
```
> print(r'\n\t\\')

\n\t\\
```
 
* 可以使用`...`做为`print('''   ''')`内的换行

例子：
```
# 这里加上r并不影响换行的转义
> print('''234234
... 234324
... 123123''')

234234
234234
123123
```

* `\` 除号结果永远是浮点数，`\\` 才是整除，`%` 仍然是取余作用

* utf-8 编码文件
告诉Python解析器按照UTF-8编码读取源文件
``` python
# -*- coding: utf-8 -*-
```

## 数据类型和变量

* 布尔值 `True False`，布尔运算符 `and or not`

* 空值 `None`，可以将某些对象的值赋为`None`

* 十六进制前缀0x、0-9、a-f表示

* 1.2x10的9次方可表示为1.2e9，0.00012可表示为1.2e-4（科学计数法）

* 理解赋值的过程：

``` python
 > a = '10'
 # 计算机首先在内存中存放'10'，然后创建一个a对象，再将a指向'10'所在位置
 > a = 10
 # 当给a赋新的值时，也是先创建值10，然后再将a指向10所在位置 
 > b = a
 > a = '111'
 #  
```

* Python的整数没有大小限制，但浮点数太大将表示为inf(无限大)

Python中表示inf
``` python
> a = float('inf')
```

* ASCII采用1个字节编码，Unicode采用2个字节编码，UTF-8采用可变长编码，可兼容ASCII

* Python3中以Unicode编码字符串

``` python
 > ord('A')
 65
 > ord('中')
 20013
 > chr(66)
 'B'
 > chr(25991)
 '文'
 > '\u4e2d\u6587'   # 直接使用Unicode字节编码的字符串
 '中文'
```

* 以单个字节的方式表示字符串，`b'...'`

```
 > 'ABC'.encode('ascii')
 b.'ABC'
 > '中文'.encode('utf-8')
 b'\xe4\xb8\xad\xe6\x96\x87'
 > '中文'.encode('gb2313') # Python还支持GB2313
 b'\xd6\xd0\xce\xc4'
 > '中文'.encode('ascii') # 中文的字符超过了ascii的表示范围
 UnicodeEncodeError                        Traceback (most recent call last)
<ipython-input-20-76f41cd8dafa> in <module>()
----> 1 '中文'.encode('ascii')

UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

* 从磁盘上读到的字节流为`b'...'`形式，可对其进行编码

```
> b'ABC'.decode('ascii')
'ABC'
> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

* 计算字符串的字符长度，`len()`

```
> len('ABCD')
4
> len('中文')
2
```

* Python中格式化输出和C语言一致，需使用 `%`

```
> 'Hello, %s' % 'World!'
Hello, World!

# %运算符用于格式化代码，%s表示字符串占位符
```

常用占位符：
%d 整数
%f 浮点数
%s 字符串
%x 十六进制整数

（注：当字符串中%为普通字符时，使用%%转义）

## 列表（list）和元组（tuple）

* list是Python内置数据类型，是一个有序集合，可添加和删除元素


```

```







2017年12月21日 11:53:03