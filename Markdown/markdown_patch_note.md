## Markdown 页内跳转

有两个地方常用到页内跳转，一个是目录，一个是文献下标。

#### 目录实现方法：

* 第一步：

目录正文

``` markdown
* [1. 标题xx](#1)
* [1.1 二级标题xx](#2)
* [1.1.1 三级标题xx](#3)
```

* 第二步：

在正文中的标题处添加

``` markdown
<h2 id="1">1. 标题xx</h2>
...
```

#### 锚点方式生成：

* 第一步：

定义一个锚（id）

``` markdown
<span id="jump1">跳转到这里</span>
```

* 第二步：

在跳转处设定跳转

``` markdown
[点击开始跳转](#jump1)
```

#### 参考

1. [MarkDown技巧：两种方式实现页内跳转](http://www.cnblogs.com/JohnTsai/p/4027229.html)