# Lua 编程学习

---

*https://www.runoob.com/lua/lua-tutorial.html*

> Lua 是一种轻量小巧的脚本语言，用标准 C 语言编写并以源代码形式开放，其设计目的是为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。Lua 是巴西里约热内卢天主教大学（Pontifical Catholic University of Rio de Janeiro）里的一个研究小组于 1993 年开发的

## 特性

- **轻量级** 它用标准 C 语言编写并以源代码形式开放，编译后仅仅一百余 K，可以很方便的嵌入别的程序里
- **可扩展** Lua 提供了非常易于使用的扩展接口和机制：由宿主语言（通常是 C 或 C++）提供这些功能，Lua 可以使用它们，就像是本来就内置的功能一样
- **其他**
  - 支持面向过程（procedure-oriented）编程和函数式编程（functional programming）
  - 自动内存管理；只提供了一种通用类型的表（table），用它可以实现数组，哈希表，集合，对象
  - 语言内置模式匹配；闭包（closure）；函数也可以看做一个值；提供多线程（协同进程，并非操作系统所支持的线程）支持
  - 通过闭包和table可以很方便地支持面向对象编程所需要的一些关键机制，比如数据抽象，虚函数，继承和重载等
  - 用在嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能

# 语法

注释

``` lua
-- 单行注释
--[[
多行注释

--]]
```

标识符

- Lua 标示符用于定义一个变量，函数获取其他用户定义的项。标示符以一个字母 A 到 Z 或 a 到 z 或下划线 _ 开头后加上 0 个或多个字母，下划线，数字（0 到 9）
- 最好不要使用下划线加大写字母的标示符，因为 Lua 的保留字也是这样的，如 `_VERSION` 被保留用于 Lua 内部全局变量
- Lua 不允许使用特殊字符如 @, $, 和 % 来定义标示符。 Lua 是一个区分大小写的编程语言

关键词：

``` lua
and  break  do  else  elseif  end  false  for  function  if  in  local  nil  not  or  repeat  return  then  true  until  while
```

全局变量不用声明，一个未赋值的全局变量，值默认为 `nil`

> Lua是动态类型语言，变量不要类型定义，只需要为变量赋值。 值可以存储在变量中，作为参数传递或结果返回

数据类型

Lua 中有 8 个基本类型分别为

- `nil` 只有值 `nil` 属于该类，表示一个无效值（在条件表达式中相当于false），给一个全局变量或表元素赋为 `nil` 即表示删除该变量
- `boolean` 包含 false 和 true 两个值
- `number` 表示双精度类型的实浮点数
- `string` 字符串由一对双引号或单引号来表示
- `function` 由 C 或 Lua 编写的函数
- `userdata` 表示任意存储在变量中的 C 数据结构，即用户自定义的类型
- `thread` 表示执行的独立线路，用于执行协同程序
- `table` Lua 中的表（table）其实是一个“关联数组”（associative arrays），数组的索引可以是数字或者是字符串。在 Lua 里，table 的创建是通过“构造表达式”来完成，最简单构造表达式是 `{}`，用来创建一个空表

``` lua
-- 打印类型
print(type('hello'))
```

``` lua
> type(X)==nil
false
> type(X)=='nil'
true

-- 原因是因为 type(type(X))==string
```

- Lua 把 false 和 nil 看作是"假"，其他的都为"真"
- Lua 默认只有一种 number 类型 -- double（双精度）类型

``` lua
print(type(2))
print(type(2.2))
print(type(0.2))
print(type(2e+1))
print(type(0.2e-1))
print(type(7.8263692594256e-06))
```

- 也可以用 2 个方括号 "[[]]" 来表示"一块"字符串

``` lua
html = [[
    ishfiewjrljoadf
    jweprfijaisfwe
    jfoweirj
]]
```

- 在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字

``` lua
> print("2" + 6)
8.0
> print("2" + "6")
8.0
> print("2 + 6")
2 + 6
> print("-2e2" * "6")
-1200.0
> print("error" + 1)
stdin:1: attempt to perform arithmetic on a string value
stack traceback:
    stdin:1: in main chunk
    [C]: in ?
>
```

- 字符串连接使用的是 ` .. `，注意两个点左右两边要有空格

``` lua
> print("a" .. 'b')
ab
> print(157 .. 428)
157428
> a .. 5
stdin:1: attempt to concatenate a nil value (global 'a')
stack traceback:
	stdin:1: in main chunk
	[C]: in ?
```

- 使用 # 来计算字符串的长度，放在字符串前面

``` lua
> print(#'haiurfhwer')
10
> str = 'werjisdfewr'
> print(#str)
11
```

- Lua 的 table 类似 Python，但是把 1 作为初始索引，table 不会固定长度大小，有新数据添加时 table 长度会自动增长，没初始的 table 都是 nil

``` lua
-- 创建一个空的 table
local tbl1 = {}

-- 直接初始表
local tbl2 = {"apple", "pear", "orange", "grape"}
```

``` lua
-- table_test2.lua 脚本文件
local tbl = {"apple", "pear", "orange", "grape"}
for key, val in pairs(tbl) do
    print("Key", key)
end

Key    1
Key    2
Key    3
Key    4
```

- function 可以以匿名函数的形式传递

``` lua
-- function_test2.lua 脚本文件
function testFun(tab,fun)
    for k ,v in pairs(tab) do
        print(fun(k,v));
    end
end


tab={key1="val1",key2="val2"};
testFun(tab,
function(key,val)--匿名函数
    return key.."="..val;
end
);
```

## 高级部分

- Lua 里，最主要的线程是协同程序（coroutine）。它跟线程（thread）差不多，拥有自己独立的栈、局部变量和指令指针，可以跟其他协同程序共享全局变量和其他大部分东西
- 线程跟协程的区别：线程可以同时多个运行，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起（suspend）时才会暂停
- userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用
