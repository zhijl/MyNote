# 学习 JavaScript 过程中的笔记

## 学习动机

为什么要学习 JavaScript 呢，因为我想要去修改一个前端项目的代码，把那个前端项目的界面改成我想要的样子。

那是一个怎么的前端项目呢？目前我了解的是它使用的是 Vue 的技术，而 Vue 用到 Node.js，Node.js 自然离不开 JavaScirpt，所以我决定先简单地熟悉一下 JavaScript 的语法。

以前一直很想跨进前端的大门，但是也仅仅止步于 HTML/CSS 的一些认识。或许这一次能更进一步吧。

由于急于速成，目前主要看的资料是 `https://www.runoob.com/js/js-tutorial.html`

## 了解 JavaScript

- HTML 定义了网页的内容
- CSS 描述了网页的布局
- JavaScript 网页的行为

可以注意到，JavaScript 为前端赋予了逻辑，是动态的。

## 语法特点

类似 Java

- 语句后 `;` 加不加都可以
- 注释风格 `//` 和 `/*...*/`
- 变量用关键词 `var` 定义，函数内生命周期为函数内部（局部变量），函数外为页面关闭后，变量类型**动态**识别，类似 Python（另：不加 var 也可以，属于隐试声明，作为 `window` 的一个属性（`window` 对象只在网页中存在），不推荐这种），`delete` 只是删除变量的属性，并不删除变量。局部变量，包括 window 对象可以覆盖全局变量和函数
- 大小写敏感
- 函数用 `function` 定义: `function funcName(arg,,,) { code lines }`
- 使用 Unicode 字符集编码
- 变量已字母、`$` 或者 `_` 开头
- 字符串 `"..."` 或者 `'...'`
- 字符串，属性: `constructor` 返回创建字符串的构造函数，`length` 长度，`prototype` 是否运行向对象添加属性和方法，方法: 很多...
- 字符串和数字相加是字符串
- `var a = new String("sdfsdffd")` 是一个对象字符串，不推荐使用，容易混淆，速度慢
- 使用未定义的变量，值为 `undefined`，没复制的变量值为 `null`
- 数组类型: `var x = new Array(); x[0] = 1;` 不用指定长度也行
- 数组也可以用字符串索引
- 对象类型类似 Python 里的字典，`var p = {a: 1, b: '234'}`，key 加不加引号都可以，key 本质上就是个变量，访问方式两种: `p.a` 或者 `p["a"]`
- 可以给对象定义方法: `methodName : function() { code lines }`
- 可以用 `new` 显试定义xx类型的变量（类型名: `String Number Boolean Array Object`）
- HTML 上的一些事件的发生可以出发 JavaScript 代码的执行，通过在 HTML 元素中添加事件属性实现
- `===` 表示绝对相等，即数据类型和值都相等
- `!==` 绝对不等于，类型和值有一个不等就不等
- `typeof varName` 返回 `varName` 变量的类型（`string number boolean object undefined`）
- `null` 也属于 `object`，它和 `undefined` 值相等，但是类型不同，`undefined` 类型也为 `undefined`，给变量赋值为 `undefined`
- 运算符基本同 Java/C++，也有三目运算符
- 循环语句，条件语句都同 Java/C++，但是 `for (var i = 0; i < a.length; ++i)` 中的 i 循环结束后依然存在
- 有这种用法 for (x in persons)
- JavaScript 标签: `labelName: { ... }`，可以用 `break` 从中途跳出
- 内存回收: 总有一个对象不再被任何变量引用时，才释放
- JavaScript 支持正则表达式
- JavaScript 变量提升问题
- `"use strict";` 该声明后，表示未显试定义的变量会报错

### 字面量

固定值成为字面量，字面量为值

- 数字（Number）字面量
- 字符串（String）字面量
- 表达式字面量
- 数组（Array）字面量，`var x = new Array(); x[0] = 1;` 或者 `var x = new Array(1, 2, 3);` 或者 `var x = [1, 2, 4]`
- 对象（Object）字面量
- 函数（Function）字面量
- 布尔值

### 变量

变量有变量名，所存储的值可变，用关键词 `var` 定义变量

``` js
var y;
var x = 10;
```

### 类型

在 JavaScript 中有 5 种不同的数据类型：

``` js
string
number
boolean
object
function
```

3 种对象类型：

``` js
Object
Date
Array
```

2 个不包含任何值的数据类型：

``` js
null
undefined
```

示例:

``` js
typeof "John"                 // 返回 string
typeof 3.14                   // 返回 number
typeof NaN                    // 返回 number
typeof false                  // 返回 boolean
typeof [1,2,3,4]              // 返回 object
typeof {name:'John', age:34}  // 返回 object
typeof new Date()             // 返回 object
typeof function () {}         // 返回 function
typeof myCar                  // 返回 undefined (如果 myCar 没有声明)
typeof null                   // 返回 object
```

### 常见的 HTML 事件

``` js
onchange      // HTML 元素改变
onclick       // 用户点击 HTML 元素
onmouseover   // 用户在一个HTML元素上移动鼠标
onmouseout    // 用户从一个HTML元素上移开鼠标
onkeydown     // 用户按下键盘按键
onload        // 浏览器已完成页面的加载
```

更多: https://www.runoob.com/jsref/dom-obj-event.html

## 实例解析

TODO: 放一个大一点的示例
