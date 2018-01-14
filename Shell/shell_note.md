# Shell 编程零碎笔记

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

## Shell 种类

- Bourne Shell（/usr/bin/sh 或 /bin/sh）
- Bourne Again Shell（/bin/bash）
- C Shell（/usr/bin/csh）
- K Shell（/usr/bin/ksh）
- Shell for Root（/sbin/sh）
- ......

 Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。后面的语法以Bash为标准

> 注：在脚本中 `#!` 是一个约定的标记，它告诉系统执行该脚本时需要通过那一个Shell解释器来执行，如`#!/bin/sh` 或 `#!/bin/bash`，指定解释器所在路径

## Shell 变量

### 定义变量

定义变量时变量名不加 $ 符，并且变量名和等号之间不能有空格

例子：

``` shell
name="zhi"
```

命名规则：

- 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头
- 中间不能有空格，可以使用下划线（_）
- 不能使用标点符号
- 不能使用bash里的关键字（可用help命令查看保留关键字）

除普通的赋值方式还可以通过某些语句进行赋值，如

``` shell
for file in `ls /etc`
```

该语句将/etc目录下的文件名循环列出

### 使用变量

- 通过在变量名前加一个 $ 符引用变量值

``` shell
name="zhi"
echo $name
echo ${name}
```

- {} 括号可以不加，但加上括号有助解析器识别（建议变量都加上{}），例如

``` shell
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```

- 变量可以被重新赋值

``` shell
name="zhi"
name="kin"
```

- 使用 `readonly` 命令可将变量变为只读变量，只读变量的值不可以被修改

``` shell
name="zhi"
readonly name
```

- 使用 `unset` 命令可以删除变量，之后改变量的值将为空

``` shell
name="zhi"
unset name
```

### 变量类型

Shell 运行时会存在三种变量

- 局部变量，局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量
- 环境变量，所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量
- shell变量，shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## Shell 字符串

- 单引号，单引号中的任何字符都将原样输出，单引号中不能出现单引号，单引号中的变量无效

- 双引号，双引号可以有变量，转义有效

``` shell
str="hello, my name is "$my_name"!\n"
```

- 字符串操作

``` shell
first_name="kin"
last_name="zhi"
echo $first_name $last_name  # 拼接字符串
```

``` shell
str="123abc"
echo ${#str}  # 计算字符串长度，输出 6
```

``` shell
str="123abc"
echo ${str:3:5}  # 截取第4个到第6个字符，输出 abc
```

``` shell
str="123abc"
echo `expr index "$str" ac` # 查找 a 或 c 所在位置，输出 3
```

## Shell 数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小

- 定义方式

```
数组名=(值1 值2 ... 值n)
```

还可以单独定义每个数组元素，下标的顺序范围没有限制

``` shell
arr[0]=value0
arr[1]=value1
arr[2]=value2
```

- 读数组

``` shell
${arr[1]}
${arr[@]}  # 获取全部元素
```

- 获取数组长度

``` shell
length=${#arr[@]}   # 获取数组元素个数
```

## Shell 注释

使用 # 进行注释

## Shell 传递参数

- 在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

``` shell
#!/bin/bash
echo "$0"
echo "$1"
echo "$3"
```

 - 其他几个特殊字符

| 参数处理       | 说明       |
| :------------ | :-------- |
| $#            | 传递到脚本的参数个数 |
| $*            | 以一个单字符串显示所有向脚本传递的参数，如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数 |
| $$            | 脚本运行的当前进程ID号 |
| $!            | 后台运行的最后一个进程的ID号 |
| $@            | 与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数 |
| $-            | 显示Shell使用的当前选项，与set命令功能相同 |
| $?            | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误 |

## Shell 运算符


## Shell echo 命令


## Shell printf 命令


## Shell test 命令


## Shell 流程控制


## Shell 函数


## Shell 输入/输出重定向


## Shell 文件包含


## 参考

[Shell 教程](http://www.runoob.com/linux/linux-shell.html)


