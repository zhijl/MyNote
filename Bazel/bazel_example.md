# Bazel C++ 例子

构建一个C++工程

## 准备工作

下载例子项目文件

```
git clone https://github.com/bazelbuild/examples/
```

项目目录结构简化如下

```
examples
└── cpp-tutorial
    ├──stage1
    │  └── main
    │      ├── BUILD
    │      ├── hello-world.cc
    │  └── WORKSPACE
    ├──stage2
    │  ├── main
    │  │   ├── BUILD
    │  │   ├── hello-world.cc
    │  │   ├── hello-greet.cc
    │  │   ├── hello-greet.h
    │  └── WORKSPACE
    └──stage3
       ├── main
       │   ├── BUILD
       │   ├── hello-world.cc
       │   ├── hello-greet.cc
       │   └── hello-greet.h
       ├── lib
       │   ├── BUILD
       │   ├── hello-time.cc
       │   └── hello-time.h
       └── WORKSPACE
```

如上面所有，目录结构分为了三个部分，也即将分为三个部分递进讲解Bazel如何组织和编译C++项目。

## 用 Bazel 编译它

#### 设置 workspace

在编译一个项目时，必须先设定了workspace。workspace就是一个文件目录，将存放项目源程序和Bazel编译后的输出，也包含几个特殊的文件：

* WORKSPACE文件
该文件将标识Bazel的工作空间，一般存放于项目的根目录
* 一个或多个BUILD文件
BUILD文件告诉Bazel如何编译项目的不同部分。（包含有BUILD文件的WORKSPACE称为package）

选定一个路径作为Bazel的workspace，同时创建一个名为WORKSPACE的文件在该路径下

#### 理解 BUILD 文件

一个BUILD文件包含几个由Bazel规则组成的部分。其中最重要的规则时build规则，告诉给Bazel如何编译出渴望的输出。例如二进制可执行文件或者链接库。在BUILD文件中的每一个build实例规则称为一个target，指向一个源文件与相依赖的文件的集合，一个target也能指向另一个target。

如cpp-tutorial/stage1/main中的BUILD文件：

```
cc_binary(
    name = "hello-world",
    srcs = ["hello-world.cc"],
)
```

在这个例子中，target名为hello-world的目标实例由Bazel的cc_binary规则实现。这条规则告诉Bazel编译一个来自hello-world.cc源文件的独立的可执行程序。

这样的规则可以明确地指出目标对象的实体状态和依赖等信息。其中name属性是必须的选项，指明了构建的对象实体，src用来指明Bazel需要被编译的源文件。

#### 开始编译项目

开始编译上面的例子，进行cpp-tutorial/stage1路径，运行

```
bazel build //main:hello-world
```

Bazel会有像下面这样的提示信息输出：

```
.........
INFO: Analysed target //main:hello-world (8 packages loaded).
INFO: Found 1 target...
Target //main:hello-world up-to-date:
  bazel-bin/main/hello-world
INFO: Elapsed time: 2.988s, Critical Path: 0.63s
INFO: Build completed successfully, 6 total actions
```

这样便完成了Bazel编译！Bazel将编译的输出结构存放于workspace路径下的bazel-bin中。

现在可以测试编译生成的二进制文件：

```
./bazel-bin/main/hello-world
```

#### 重现依赖图

每一个合法的build文件都将指明依赖关系，Bazel通过构建依赖图来增加编译的可靠性。

通过下面指令可生成依赖图

```
bazel query --nohost_deps --noimplicit_deps 'deps(//main:hello-world)' \
  --output graph
```

输出

```
digraph mygraph {
  node [shape=box];
"//main:hello-world"
"//main:hello-world" -> "//main:hello-world.cc"
"//main:hello-world.cc"
}
```

上面的指令将生成hello-world目标的依赖图

将上述命令输出的信息复制到该网站http://www.webgraphviz.com/，便可得到可视化图像

#### 优化 Bazel 编译

在cpp-tutorial/stage2/main中展现了一个有依赖关系的BUILD

```
cc_library(
    name = "hello-greet",
    srcs = ["hello-greet.cc"],
    hdrs = ["hello-greet.h"],
)

cc_binary(
    name = "hello-world",
    srcs = ["hello-world.cc"],
    deps = [
        ":hello-greet",
    ],
)
```

在这个BUILD文件中，Bazel首先编译hello-greet这个链接库，使用Bazel内置的cc_library规则，然后编译hello-world库

