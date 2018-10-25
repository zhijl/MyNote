# Bazel 学习笔记

## Bazel 是什么

Bazel 是一个构建工具，即一个可以运行编译和测试来组装软件的工具，跟 Make、Ant、Gradle、Buck、Pants 和 Maven 一样

## Bazel 特点

Bazel 是设计用来配合 Google 的软件开发模式。有以下几个特点：

* 多语言支持：Bazel 支持 Java，Objective-C 和 C++，可以扩展来支持任意的编程语言
* 高级别的构建语言：工程是通过 BUILD 语言来描述的。BUILD 语言以简洁的文本格式，描述了由多个小的互相关联的库、二进制程序和测试程序来组成的一个项目。而与之相比，Make 这类的工具需要描述各个单独的文件和编译的命令
* 多平台支持：同一套工具和同样的 BUILD 文件可以用来构建不同架构和不同平台的软件。在 Google，我们使用 Bazel 来构建在我们数据中心系统中运行的服务器端程序和在手机上运行的客户端应用程序。
* 重现性 [Reproducibility]：在 BUILD 文件中，每个库，测试程序，二进制文件必须明确完整地指定直接依赖。当修改源代码文件后，Bazel 使用这个依赖信息就可以知道哪些必须重新构建，哪些任务可以并行执行。这意味者所有的构建都是增量形式的并能够每次都生成相同的结果。
* 伸缩性 [Scalability]：Bazel 可以处理巨大的构建；在 Google，一个服务器端程序超过 100k 的源码是常有的事情，如果没有文件被改动，构建过程需要大约 200ms

## 为什么使用 Bazel

* Make,Ninja: 通过这些工具都能够控制执行哪些命令来构建文件，但是需要用户书写正确的规则。

用户跟Bazel在更高级别上交互。例如，它有内置的"Java test", "C++ binary"的规则[rule]，有例如“目标平台”[target platform],“主机平台"[host platform]这种标记。这些规则都经历了充分的测试，是不会出错的。

* Ant 和 Maven： Ant 和 Maven 主要是面向 Java，而 Bazel 可以处理多种语言。Bazel 鼓励把代码库的内容划分成小的，可复用的单元，并且只重新构建需要重新构建的文件。这会提高在庞大的代码库上开发的速度。
* Gradle: Bazel 配置文件比 Gradle 的要更加结构化，让 Bazel 能够准确理解每个行为的所作所为。使得能够有更多的并发和更好的可重现性
* Pants, Buck: 这两个工具都是前 Google 员工在 Twitter 和 Foursquare 创造并开发的。他们都是在模仿 Bazel，但是他们的特性跟 Bazel 是不同的，所以不会是 Bazel 的替代品

## Bazel 起源

Bazel 是 Google 内部用来构建自己的服务器端软件的工具。它已经被扩展成也可以构建连接到服务器端的客户端软件(iOS, Android)

## Bazel 最擅长的

* 有庞大代码库的项目
* 用（多种）需要编译的语言写的项目
* 在多平台上部署的项目
* 有大量测试的工程

## 参考

[Google软件构建工具Bazel](https://www.cnblogs.com/Leo_wl/p/4458115.html)