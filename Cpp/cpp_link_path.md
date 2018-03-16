# 链接选项 `-I,-l,-L,-Wl:rpath` 区别

首先这几个命令一般用在gcc/g++或makefile中，但是一般的IDE里也会涉及。只是在IDE里的配置方式会不同，有的是配置文件如QtCreator，有的是图形化界面，如CodeBlocks。无论是什么样的方式其本质都是一样的，尤其是配一个工程的时候，这些都是必不可少的。

## `-I` 添加包含路径

-I 在编译时用，告诉编译器去哪个路径下找文件

如：

``` shell
-I /home/hello/include
```

表示将/home/hello/include目录作为第一个寻找头文件的目录。

编译器的寻找顺序是：/home/hello/include-->/usr/include-->/usr/local/include。如果在/home/hello/include中有个文件hello.h，则在程序中用#include<hello.h>就能引用到这个文件。

可以加多个包含路径，编译器的寻找顺序为添加的顺序。

## `-l` 添加引用链接库

-l 在链接时用到，它的作用是告诉链接器，要用到哪个库。
如：

``` shell
-l pthread
```

告诉链接器(linker)，程序需要链接pthread这个库,这里的pthread是库名不是文件名，具体来说文件句是libpthread.so。

## `-L` 添加链接库路径
-L 后跟路径，告诉链接器从哪找库(.so文件)，只有在链接时会用到。

如：

``` shell
-L /home/hello/lib
```

表示将/home/hello/lib目录作为第一个寻找库文件的目录，寻找顺序是：

``` shell
/home/hello/lib-->/usr/lib-->/usr/local/lib。
```

可以加多个包含路径，链接器的寻找顺序为添加的顺序。

## `-Wl:rpath` 添加运行时库路径

-Wl:rpath 后面也是路径，运行的时候用。这条编译指令会在编译时记录到target文件中，所以编译之后的target文件在执行时会按这里给出的路径去找库文件。

如：

``` shell
-Wl:rpath=/home/hello/lib
```

表示将/home/hello/lib目录作为程序运行时第一个寻找库文件的目录，程序寻找顺序是：

``` shell
/home/hello/lib-->/usr/lib-->/usr/local/lib。
```

可以加多个包含路径，程序在运行时的寻找顺序为添加的顺序。

一开始我认为编译路径和运行路径应该是一样的才对，为什么要分两个？实际上在配置工程的时候为了标准化，编译出的target（.so或是可执行文件）不一定跟源文件（main.cpp,XX.cpp）放在一个文件夹下。如不在一个文件夹下，在编译时，-L后面跟的是相对于源文件的路径，-Wl:rpath后面跟的是相对于target（.so或是可执行文件）的路径，确切来说是相对于工作目录的路径。QtCreator的工作路径就是target文件的路径，Codebloks的工作路径是.cbp文件的路径。所以不同的IDE在配置有时候会一些细微不同，这些不同一不注意就会有大坑。
       估计CodeBlocks在找可执行文件时是用../../这种方式过去的，这样就能在.cbp的文件路径下执行可执行文件。如果用cd会改变工作路径。

## 参考

[链接选项`-I,-l,-L,-Wl:rpath`](http://blog.csdn.net/xianxjm/article/details/73609142)
