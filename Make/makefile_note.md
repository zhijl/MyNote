# 写一个完美的 Makefile （一些准备工作）

### Makefile 核心规则

``` Makefile
target ... : prerequisites ...
    command
    ...
    ...
```

- **target** 可以是一个 object file（目标文件），也可以是一个执行文件，还可以是一个标签（label）
- **prerequisites** 生成该 target 所依赖的文件或 target
- **command** 该target要执行的命令（任意的shell命令，make 使用 UNIX 的标准 shell，即 /bin/sh  执行命令）

定义了 target 这样一个或者多个目标文件所依赖的一个或多个文件 prerequisites，以及他们具体的关系体现 command

prerequisites 中一个或多个文件更新时，会使 target 也重新生成

### 包含其他 Makefile

``` Makefile
include <filename>
# 例如
include foo.make *.mk $(bar)
```

也可以在 include 前加一个 -，这样如果 filename 无法正常获取到，make 不会中断执行，而是继续后面的执行

### make 的工作方式

1. 读入所有的 Makefile
2. 读入被 include 的其它 Makefile
3. 初始化文件中的变量
4. 推导隐晦规则，并分析所有规则
5. 为所有的目标文件创建依赖关系链
6. 根据依赖关系，决定哪些目标要重新生成
7. 执行生成命令

1-5步为第一个阶段，6-7为第二个阶段。第一个阶段中，如果定义的变量被使用了，那么，make 会把其展 开在使用的位置。但 make 并不会完全马上展开，make 使用的是拖延战术，如果变量出现在依赖关系的规则 中，那么仅当这条依赖被决定要使用了，变量才会在其内部展开。

### 文件搜寻

- 通过 make 内置变量设置搜索路径

``` Makefile
VPATH = src:../headers
```

- 通过 vpath 关键词（命令）设置搜索路径

``` Makefile
# 为符合模式 <pattern> 的文件指定搜索目录 <directories>
# 1. <pattern> 需要包含 % 字符
# 2. % 的意思是匹配零或若干字符
# 3. 多个目录使用 : 隔开
vpath <pattern> <directories>

# 清除符合模式 <pattern> 的文件的搜索目录
vpath <pattern>

# 清除所有已被设置好了的文件搜索目录
vpath

# 例如
vpath %.h ../headers
vpath %.c foo:bar
vpath %   blish
```

### 伪目标

使用一个特殊的标记 `.PHONY` 来显式地指明一个目标是伪目标

例如：

``` Makefile
.PHONY : clean
```

伪目标同样可以作为 “默认目标”，只要将其放在第一个

### 静态模式

在核心规则上加入了一些灵活的模式

``` Makefile
<targets ...> : <target-pattern> : <prereq-patterns ...>
    <commands>
    ...
```

- **targets** 定义了一系列的目标文件，可以有通配符。是目标的一个集合
- **target-parrtern** 是指明了 targets 的模式，也就是的目标集模式
- **prereq-parrterns** 是目标的依赖模式，它对 target-parrtern 形成的模式再进行一次依赖目标的定义

例如：

``` Makefile
objects = foo.o bar.o

all: $(objects)

$(objects): %.o: %.c
    $(CC) -c $(CFLAGS) $< -o $@
```

依赖文件中含模式 %，那么目标文件中也要含 % 模式

### 变量

#### 获取值

变量中存放的是文本字符串，大小写敏感

获取变量的值（最好加上括号）：

``` Makefile
$(VAR) 或者 ${VAR}
```

如果要使用 $，则通过 $$ 进行转义

#### 赋值

`=` 或者 `:=`

`=` 特点如下：

``` Makefile
a = b
b = c
c = name.c
```

即变量可以使用后面定义的变量提前赋值，而 `:=` 恰好屏蔽了这种特性

#### 定义一个值为空格的变量

``` Makefile
nullstring :=
space := $(nullstring) # end of the line
```

这里的 `space` 的值就是一个空格，而 `nullstring` 是值为空

#### `?=` 操作符

``` Makefile
FOO ?= bar
```

如果 `FOO` 先前被定义过，那么这条语句将不做任何赋值，否则 `FOO` 的值为 bar

这种方式等价于

``` Makefile
ifeq ($(origin FOO), undefined)
    FOO = bar
endif
```

#### 变量的高级篇

- 替换变量中共同的某一部分

``` Makefile
$(var:a=b)
或者
${var:a=b}
```

即把 `var` 变量中所有以 a 结尾的词全部替换为以 b 结尾

例如：

``` Makefile
foo := a.o b.o c.o
bar := $(foo:.o=.c)
```

- 把变量的值当成变量

``` Makefile
x = y
y = z
a := $($(x))
```

甚至还可以

``` Makefile
x = y
y = z
z = u
a := $($($(x)))
```

`a` 的值为 u

- 两个变量值组合重命名变量

``` Makefile
first_second = Hello
a = first
b = second
all = $($a_$b)
```

- 追加变量

``` Makefile
VAR += value1
```



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

### 小技巧

1. 加 `-`

``` Makefile
.PHONY : clean
clean :
    -rm edit $(objects)
```

在 rm 前面加上 - ，意思就是，也许某些文件出现问题，但不要管，继续做后面的事。clean 的规则不要放在文件的开头，不然，这就会变成 make 的默认目标，当然也可以在 make 执行时加入 -k 或 --keep-going 参数

2. 尽量不要定义和使用 MAKEFILES 变量

3. 避免晦涩的隐式规则

4. make 中使用通配符，make 支持三个通配符，即 `*` `?` `~`

`~` 表示系统主目录
`*` 任意长度的任意字符
`?` 表示任意的一个字符

5. 轻松列出源文件所依赖的头文件

使用编译器的 `-M` 或 `-MM` 选项

``` shell
gcc -M main.c

main.o: main.c defs.h /usr/include/stdio.h /usr/include/features.h \
    /usr/include/sys/cdefs.h /usr/include/gnu/stubs.h \
    /usr/lib/gcc-lib/i486-suse-linux/2.95.3/include/stddef.h \
    /usr/include/bits/types.h /usr/include/bits/pthreadtypes.h \
    /usr/include/bits/sched.h /usr/include/libio.h \
    /usr/include/_G_config.h /usr/include/wchar.h \
    /usr/include/bits/wchar.h /usr/include/gconv.h \
    /usr/lib/gcc-lib/i486-suse-linux/2.95.3/include/stdarg.h \
    /usr/include/bits/stdio_lim.h
```

``` shell
gcc -MM main.c

main.o: main.c defs.h
```

但是这样的依赖关系列表依然不容易在 Makefile 中很好的使用，不过 GNU 组织建议让编译器为每一个源文件的自动生成的依赖关系放到一个文件中，为每一个 name.c 的文件都生成一个 name.d 的 Makefile 文件，.d 文件中存放对应 .c 文件的依赖关系

例如：

``` Makefile
%.d: %.c
    @set -e; rm -f $@; \
    $(CC) -M $(CPPFLAGS) $< >; $@.$$$$; \
    sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ >; $@; \
    rm -f $@.$$$$
```

`$$$$` 是生成一个随机编码

6. 在命令前加 @，这样 make 将不会把命令语句也输出，或者执行 make 时加入 -s | --silent | --quiet 参数也能全面禁止命令显示



引用：

1. [Makefile常用函数总结](https://blog.csdn.net/ustc_dylan/article/details/6963248)
2. [跟我一起写Makefile](https://seisman.github.io/how-to-write-makefile/rules.html)