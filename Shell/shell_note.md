# Shell 编程零碎笔记

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

## linux cp 命令直接覆盖不提示 y/n 的方法

在 docker 容器里操作时有时候会遇到提示 y/n 的情况，这主要是系统的某个配置文件中对 cp 命令设置了别名，即 `alias cp='cp -i'`

想要不提示 y/n 而直接覆盖拷贝，则执行 `alias cp=cp` 覆盖原别名设置即可

## 一些小工具和 trick

``` sh
strace caffe.bin   # 可以看到很多程序的系统状态信息
```

``` sh
objdump -s --section=.comment caffe.bin    # Linux 上查看二进制库和文件是哪个版本的编译器编译的（也可能查不到）
```

## 获取执行结果返回值

``` shell
command1
echo $?
```

`$?` 即为上一个命令的退出时的状态码

``` shell
command1 > /dev/null
echo $?
```

参考：https://blog.csdn.net/yxys01/article/details/76548934

## 退出脚本或函数并返回错误码

退出脚本可使用 `exit n`，错误码即 `n`

退出码（exit status，或exit code）的约定：

- 0 表示成功（Zero - Success）

- 非 0 表示失败（Non-Zero  - Failure）

- 2 表示用法不当（Incorrect Usage）

- 127 表示命令没有找到（Command Not Found）

- 126 表示不是可执行的（Not an executable）

- >= 128 信号产生

函数的退出使用 `return n`

条件语句的退出使用 `break`

## Shell 标准错误输出重定向

``` sh
/tmp/test.sh > /tmp/test.log 2>&1
```

参考：https://www.cnblogs.com/vijayfly/p/6234575.html

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

| 参数处理 | 说明                                                                                                                 |
| :------- | :------------------------------------------------------------------------------------------------------------------- |
| $#       | 传递到脚本的参数个数                                                                                                 |
| $*       | 以一个单字符串显示所有向脚本传递的参数，如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数                  |
| $$       | 脚本运行的当前进程ID号                                                                                               |
| $!       | 后台运行的最后一个进程的ID号                                                                                         |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数 |
| $-       | 显示Shell使用的当前选项，与set命令功能相同                                                                           |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误                                                          |

## Shell 运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用

- expr 是一款表达式计算工具，使用它能完成表达式的求值操作

``` shell
a=2
val=`exp $a + 3`
echo "$val"
```

> 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2
> 完整的表达式要被反引号 ` ` 包含

- 算术运算示例

``` shell
#!/bin/bash
a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

> - 乘号(*)前边必须加反斜杠(\)才能实现乘法运算
> -  MAC 中 shell 的 expr 语法是：$((表达式))，此处表达式中的乘号 "*" 不需要转义符号 "\"

- 关系运算符示例

关系运算符只支持数字，不支持字符串，除非字符串的值是数字

| 运算符 | 说明                                                | 举例                         |
| :----- | :-------------------------------------------------- | :--------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true                   | ` [ $a -eq $b ] ` 返回 false |
| -eq    | 检测两个数是否相等，相等返回 true                   | ` [ $a -eq $b ] ` 返回 false |
| -ne    | 检测两个数是否相等，不相等返回 true                 | ` [ $a -ne $b ] ` 返回 true  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true     | ` [ $a -gt $b ] ` 返回 false |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true     | ` [ $a -lt $b ] ` 返回 true  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true | ` [ $a -ge $b ] ` 返回 false |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true | ` [ $a -le $b ] ` 返回 true  |

- 布尔运算符

| 运算符 | 说明                                              | 举例                                     |
| :----- | :------------------------------------------------ | :--------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true | ` ! false ] ` 返回 true                  |
| -o     | 或运算，有一个表达式为 true 则返回 true           | ` $a -lt 20 -o $b -gt 100 ] ` 返回 true  |
| -a     | 与运算，两个表达式都为 true 才返回 true           | ` $a -lt 20 -a $b -gt 100 ] ` 返回 false |

- 逻辑运算符


| 运算符 | 说明       | 举例                                          |
| :----- | :--------- | :-------------------------------------------- |
| `&&`   | 逻辑的 AND | ` [[ $a -lt 100 && $b -gt 100 ]] ` 返回 false |
| `||`   | 逻辑的 OR  | ` [[ $a -lt 100 || $b -gt 100 ]] ` 返回 true  |

- 字符串运算符

| 运算符                                  | 说明                                    | 举例                     |
| :-------------------------------------- | :-------------------------------------- | :----------------------- |
| =	检测两个字符串是否相等，相等返回 true | `[ $a = $b ]` 返回 false                |
| !=                                      | 检测两个字符串是否相等，不相等返回 true | `[ $a != $b ]` 返回 true |
| -z                                      | 检测字符串长度是否为0，为0返回 true     | `[ -z $a ]` 返回 false   |
| -n                                      | 检测字符串长度是否为0，不为0返回 true   | `[ -n $a ]` 返回 true    |
| str                                     | 检测字符串是否为空，不为空返回 true     | `[ $a ]` 返回 true       |

- 文件测试运算符

| 操作符  | 说明                                                                      | 举例                    |
| :------ | :------------------------------------------------------------------------ | :---------------------- |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true                             | [ -b $file ] 返回 false |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true                           | [ -c $file ] 返回 false |
| -d file | 检测文件是否是目录，如果是，则返回 true                                   | [ -d $file ] 返回 false |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true | [ -f $file ] 返回 true  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true                           | [ -g $file ] 返回 false |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true                 | [ -k $file ] 返回 false |
| -p file | 检测文件是否是有名管道，如果是，则返回 true                               | [ -p $file ] 返回 false |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true                           | [ -u $file ] 返回 false |
| -r file | 检测文件是否可读，如果是，则返回 true                                     | [ -r $file ] 返回 true  |
| -w file | 检测文件是否可写，如果是，则返回 true                                     | [ -w $file ] 返回 true  |
| -x file | 检测文件是否可执行，如果是，则返回 true                                   | [ -x $file ] 返回 true  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true                    | [ -s $file ] 返回 true  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true                         | [ -e $file ] 返回 true  |

补充：

```
 Shell中判断语句if中-z至-d的意思

[ -a FILE ] 如果 FILE 存在则为真。

[ -b FILE ] 如果 FILE 存在且是一个块特殊文件则为真。

[ -c FILE ] 如果 FILE 存在且是一个字特殊文件则为真。

[ -d FILE ] 如果 FILE 存在且是一个目录则为真。

[ -e FILE ] 如果 FILE 存在则为真。

[ -f FILE ] 如果 FILE 存在且是一个普通文件则为真。

[ -g FILE ] 如果 FILE 存在且已经设置了SGID则为真。

[ -h FILE ] 如果 FILE 存在且是一个符号连接则为真。

[ -k FILE ] 如果 FILE 存在且已经设置了粘制位则为真。

[ -p FILE ] 如果 FILE 存在且是一个名字管道(F如果O)则为真。

[ -r FILE ] 如果 FILE 存在且是可读的则为真。

[ -s FILE ] 如果 FILE 存在且大小不为0则为真。

[ -t FD ] 如果文件描述符 FD 打开且指向一个终端则为真。

[ -u FILE ] 如果 FILE 存在且设置了SUID (set user ID)则为真。

[ -w FILE ] 如果 FILE 如果 FILE 存在且是可写的则为真。

[ -x FILE ] 如果 FILE 存在且是可执行的则为真。

[ -O FILE ] 如果 FILE 存在且属有效用户ID则为真。

[ -G FILE ] 如果 FILE 存在且属有效用户组则为真。

[ -L FILE ] 如果 FILE 存在且是一个符号连接则为真。

[ -N FILE ] 如果 FILE 存在 and has been mod如果ied since it was last read则为真。

[ -S FILE ] 如果 FILE 存在且是一个套接字则为真。

[ FILE1 -nt FILE2 ] 如果 FILE1 has been changed more recently than FILE2, or 如果 FILE1 exists and FILE2 does not则为真。

[ FILE1 -ot FILE2 ] 如果 FILE1 比 FILE2 要老, 或者 FILE2 存在且 FILE1 不存在则为真。

[ FILE1 -ef FILE2 ] 如果 FILE1 和 FILE2 指向相同的设备和节点号则为真。

[ -o OPTIONNAME ] 如果 shell选项 “OPTIONNAME” 开启则为真。

[ -z STRING ] “STRING” 的长度为零则为真。

[ -n STRING ] or [ STRING ] “STRING” 的长度为非零 non-zero则为真。

[ STRING1 == STRING2 ] 如果2个字符串相同。 “=” may be used instead of “==” for strict POSIX compliance则为真。

[ STRING1 != STRING2 ] 如果字符串不相等则为真。

[ STRING1 < STRING2 ] 如果 “STRING1” sorts before “STRING2” lexicographically in the current locale则为真。

[ STRING1 > STRING2 ] 如果 “STRING1” sorts after “STRING2” lexicographically in the current locale则为真。

[ ARG1 OP ARG2 ] “OP” is one of -eq, -ne, -lt, -le, -gt or -ge. These arithmetic binary operators return true if “ARG1” is equal to, not equal to, less than, less than or equal to, greater than, or greater than or equal to “ARG2”, respectively. “ARG1” and “ARG2” are integers.

摘自：https://www.cnblogs.com/coffy/p/5748292.html
```

## Shell 基础命令

- echo 回显字符串

``` shell
echo "this is a string!\n"
echo this is a stiring!\n  # 引号可以去掉
echo -e "OK! \c" # -e 开启转移 \c 不换行
echo "this is a test" > file_name # 将字符串内容重定向到文件
echo `data` # 显示命令执行结果
```
- read 读字符串

从终端读取字符串

``` shell
read name
echo "$name"
```

- printf 命令

printf 命令模仿 C 程序库（library）里的 printf() 程序，移植性比echo好

``` shell
# 单引号与双引号效果一样 
printf '%d %s\n' 1 "abc" 

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n" 
```

转义字符：
| 字符  | 说明                                                                                                                                                                         |
| :---- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \a    | 警告字符，通常为ASCII的BEL字符                                                                                                                                               |
| \b    | 后退                                                                                                                                                                         |
| \c    | 抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略 |
| \f    | 换页（formfeed）                                                                                                                                                             |
| \n    | 换行                                                                                                                                                                         |
| \r    | 回车（Carriage return）                                                                                                                                                      |
| \t    | 水平制表符                                                                                                                                                                   |
| \v    | 垂直制表符                                                                                                                                                                   |
| \\    | 一个字面上的反斜杠字符                                                                                                                                                       |
| \ddd  | 表示1到3位数八进制值的字符。仅在格式字符串中有效                                                                                                                             |
| \0ddd | 表示1到3位的八进制值字符                                                                                                                                                     |

- test 命令

## Shell 流程控制


## Shell 函数


## Shell 输入/输出重定向


## Shell 文件包含

## sed 简单使用

加 -i 文件内直接替换

在文件尾追加（下面例子中追加字符串为“TAIL”）

``` sh
sed 's/$/&TAIL/g' test.file
```

在文件头追加（下面例子中追加字符串为“HEAD”）

``` sh
sed 's/^/HEAD&/g' test.file
```

## 参考

[Shell 教程](http://www.runoob.com/linux/linux-shell.html)


2018年1月15日 00:22:27
