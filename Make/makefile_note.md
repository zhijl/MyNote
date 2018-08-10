# 写一个完美的 Makefile （一些准备工作）

### 变量



### 函数的调用

格式：

``` Makefile
# 多个参数用逗号隔开
$(<function> <arguments>)
# 或者
${<function> <arguments>}
```

### 常用函数

#### 字符串处理类

**字符串替换函数** - subst

格式：

``` Makefile
# 把字串 <text> 中的 <from> 字符串替换成 <to>
# 函数返回被替换过后的字符串
$(subst <from>, <to>, <text>)

# 例子，将 sdfjaadfwaa 中的 aa 替换为 AA
$(subst aa, AA, sdfjaadfwaa)
```

**模式字符串替换函数** - patsubst

``` Makefile
# 查找 <text> 中的单词（单词以 “空格”、“Tab” 或 “回车” “换行” 分隔）是否符合模式 <pattern>，如果匹配的话，则以 <replacement> 替换。这里，<pattern> 可以包括通配符 “%”，表示任意长度的字串。如果 <replacement> 中也包含 “%”，那么，<replacement> 中的这个 “%” 将是 <pattern> 中的那个 “%” 所代表的字串。（可以用 “\” 来转义，以 “\%” 来表示真实含义的 “%” 字符）
$(patsubst <pattern>, <replacement>, <text>)

# 例子，结果为 x.c.o bar.o
$(patsubst %.c, %.o, x.c.c bar.c)

# 结果为 x.c.c bar.c
$(patsubst .c, .o, x.c.c bar.c)
```

``` Makefile
$(var:<pattern>=<replacement>)
# 相当于
$(patsubst <pattern>, <replacement>, $(var))
```

``` Makefile
# 将变量 var 的值的后缀替换为 <replacement>
$(var:<suffix>=<replacement>)
# 相当于
$(patsubst %<suffix>, %<replacement>, $(var))
```

**去空格函数** - strip

``` Makefile
# 去掉 <string> 字串中开头和结尾的空字符
$(strip <string>)

# 例子
$(strip abcdefg)
```

**查找字符串函数** - findstring

``` Makefile
# 在字串 <in> 中查找 <find> 字串
# 如果找到，返回 <find>，否则返回空字符串
$(findstring <find>, <in>)

# 例子
$(findstring a, a bc)
```

**过滤函数** - filter

``` Makefile
# 以 <pattern> 模式过滤 <text> 字符串中的单词，保留符合模式 <pattern> 的单词。可以有多个模式，多个模式用空格分隔
# 返回经 <pattern> 模式过滤后的字符串
$(filter <pattern...>, <text>)

# 例子
$(filter %.c %.cpp, $(sources))
```

**反过滤函数** - filter-out

``` Makefile
# 以 <pattern> 模式过滤 <text> 字符串中的单词，去除符合模式 <pattern> 的单词。可以有多个模式
# 返回不符合 <pattern> 模式的字符串
$(filter <pattern...>, <text>)

# 例子，过滤 abc.cpp 文件
$(filter abc.cpp, $(sources))
```

**排序函数** - sort

``` Makefile
# 给字符串 <list> 中的单词排序（字典序升序），sort 函数会去掉 <list> 中相同的单词
$(sort <list>)
```

**取单词函数** - word

``` Makefile
# 取字符串<text>中第<n>个单词
$(word <n>, <text>)
```

**取单词串函数** - wordlist

``` Makefile
# 从字符串 <text> 中取从 <s> 开始到 <e> 的单词串。<s> 和 <e> 是一个数字。如果 <s> 比 <text> 中的单词数要大，那么返回空字符串。如果 <e> 大于 <text> 的单词数，那么返回从 <s> 开始，到 <text> 结束的单词串
$(wordlist <s>, <e>, <text>)
```

**单词个数统计函数** - words

``` Makefile
# 统计 <text> 中字符串中的单词个数，返回单词个数
$(words <text>)
```

**首单词函数** - firstword

``` Makefile
# 取字符串 <text> 中的第一个单词
$(firstword <text>)
```

#### 文件名操作函数

**取目录函数** - dir

``` Makefile
# 从文件名序列 <names> 中取出目录部分。目录部分是指最后一个反斜杠（“/”）之前的部分。如果没有反斜杠，那么返回 “./”
$(dir <names...>)
```

**取文件函数** - notdir

``` Makefile
# 从文件名序列 <names> 中取出非目录部分。非目录部分是指最后一个反斜杠（“/”）之后的部分
$(notdir <names...>)
```

**取后缀函数** - suffix

``` Makefile
# 从文件名序列<names>中取出各个文件名的后缀，包括点"."
$(suffix <names...>)
```

**取前缀函数** - basename

``` Makefile
# 从文件名序列 <names> 中取出各个文件名的前缀部分
$(basename <names...>)
```

**加后缀函数** - addsuffix

``` Makefile
# 把后缀 <suffix> 加到 <names> 中的每个单词后面
$(addsuffix <suffix>, <names...>)
```

**加前缀函数** - addprefix

``` Makefile
# 把前缀 <prefix> 加到 <names> 中的每个单词前面
$(addprefix <prefix>, <names...>)
```

**连接函数** - join

``` Makefile
# 把 <list2> 中的单词对应地加到 <list1> 的单词后面。如果 <list1> 的单词个数要比 <list2> 的多，那么，<list1> 中的多出来的单词将保持原样。如果 <list2> 的单词个数要比 <list1> 多，那么，<list2> 多出来的单词将被复制到 <list2> 中
$(join <list1>, <list2>)
# 例子
$(join aaa bbb, 111 222 333)
```

#### foreach 函数

``` Makefile
# 把参数 <list> 中的单词逐一取出放到参数 <var> 所指定的变量中，然后再执行 <text> 所包含的表达式
$(foreach <var>, <list>, <text>)
```

#### if 函数

条件函数

格式

``` Makefile
$(if <condition>, <then-part>)
# 或者
$(if <condition>, <then-part>, <else-part>)
```

#### call 函数

可以用 call 函数来向这个表达式传递参数

``` Makefile
$(call <expression>, <parm1>, <parm2>, <parm3>...)

# 例子
reverse = $(1) $(2)
foo = $(call reverse, a, b)
```

#### origin 函数

origin 会告诉你变量的定义类型

``` Makefile
# 返回
$(origin <varible>)
# 例子
$(origin VAR)
```

变量的定义类型

``` Makefile
undefined     # 如果 <variable> 从来没有定义过
default       # 如果 <variable> 是一个默认的定义，比如 “CC” 这个变量
environment   # 如果 <variable> 是一个环境变量，并且当 Makefile 被执行时，“-e” 参数没有被打开
file          # 如果 <variable> 这个变量被定义在Makefile中
override      # 如果 <variable> 是被 override 指示符重新定义的
automatic     # 如果 <variable> 是一个命令运行中的自动化变量
```

#### shell 函数

shell 函数的参数就是 shell 命令

``` Makefile
$(shell <shell-commond>)
```

引用：

1. [Makefile常用函数总结](https://blog.csdn.net/ustc_dylan/article/details/6963248)