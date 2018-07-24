# [转] multiprocessing

原文见1.和2.，在此基础上进行了适当补充和整理

---

> Python 并发库，用于解决 Python 程序中充分利用多核 CPU 资源的问题，multiprocessing 赋予每个进程单独的 Python 解释器，规避了全局解释锁带来的问题。multiprocessing 支持子进程/通信和共享数据/执行不同形式的同步，相对于 threading 这个并发库，拟补了其劣势

## 基本知识

**多道程序（multiprogramming）**：CPU读取多个程序放入内存，运行第一个程序直到它出现IO操作（或运行结束），切换到第二个程序。每个程序的时间分配不均等，可能第一个程序运行很久也没有IO操作或结束，第二个程序一直得不到运行

**多任务处理（multitasking）**：运行第一个程序一段时间，保存工作环境，切换到第二个程序运行一段时间，保存工作环境；恢复第一个程序的工作环境，继续运行第一个程序，如此往复，每个程序的运行时间相对平均

**分时系统（time sharing）**：利用多道程序或多任务处理技术，计算机处理多个用户指令时，将运行时间分成多个片段分配给每个用户指定的任务，轮流执行所有任务直至完成

**进程（process）**：程序是指令和数据的有序集合，是一个静态的概念，只有处理器调度它时才成为一个活动的实体，称为进程。进程是动态产生，动态消亡的。进程由进程控制块、程序段、数据段组成。进程控制块（Process Control Block，PCB）是操作系统中主要表示进程状态等信息的一种数据结构，是系统感知进程存在的唯一标识

**进程切换**：是进程把放在处理器寄存器的中间数据（也叫上下文）放回私有堆栈，让下一个待运行进程占用处理器，下一个进程将自己的中间数据加载到处理器寄存器，并从上一次的断点继续执行的过程

对于 Linux 系统，系统开机时建立的 init 进程的 pid 为 1，其他所有进程都由 init 进程（或其子进程）fork 而来，所有进程以 init 进程为根构成一个进程树（可以通过pstree命令查看）。fork 进程的时候，系统在内存中开辟一段新空间给新进程，然后两个进程同时运行。fork 函数与普通函数不同，有两次返回，将子进程的 PID 返回给父进程，将 0 返回给子进程，通常调用 fork 后，会设计一个 if 结构，如果返回值为 0，说明处于子进程，编码处理子进程工作，如果返回值不为0，说明处于父进程，编码处理父进程工作

> Python 由于全局锁 GIL (Global Interpreter Lock)的存在，无法享受多线程带来的性能提升。multiprocessing包采用子进程的技术避开了 GIL，使用multiprocessing 可以进行多进程编程提高程序效率

## 什么是 Python 全局锁（GIL）问题

#### CPU-bound(计算密集型)

计算密集型任务 (CPU-bound) 的特点是要进行大量的计算，占据着主要的任务，消耗 CPU 资源，一直处于满负荷状态。比如复杂的加减乘除、计算圆周率、对视频进行高清解码等等，全靠 CPU 的运算能力。这种计算密集型任务虽然也可以用多任务完成，但是任务越多，花在任务切换的时间就越多，CPU 执行任务的效率就越低，所以，要最高效地利用 CPU，计算密集型任务同时进行的数量应当等于 CPU 的核心数

计算密集型任务由于主要消耗 CPU 资源，因此，代码运行效率至关重要。Python 这样的脚本语言运行效率很低，完全不适合计算密集型任务。对于计算密集型任务，最好用 C 语言编写

#### I/O bound(I/O密集型)

IO 密集型任务 (I/O bound) 的特点是指磁盘 IO、网络 IO 占主要的任务，CPU 消耗很少，任务的大部分时间都在等待 IO 操作完成（因为 IO 的速度远远低于 CPU 和内存的速度）

IO 密集型任务执行期间，99% 的时间都花在 IO 上，花在 CPU 上的时间很少，因此，用运行速度极快的 C 语言替换用 Python 这样运行速度极低的脚本语言，完全无法提升运行效率

对于 IO 密集型任务，任务越多，CPU 效率越高，但也有一个限度。常见的大部分任务都是 IO 密集型任务，比如请求网页、读写文件等。当然我们在 Python 中可以利用 sleep 达到 IO 密集型任务的目的

对于 IO 密集型任务，最合适的语言就是开发效率最高（代码量最少）的语言，脚本语言是首选，C 语言最差

#### 全局锁 (GIL) 问题

解释器被一个全局解释器锁保护着，它确保任何时候都只有一个 Python 线程执行

GIL 最大的问题就是 Python 的多线程程序并不能利用多核 CPU 的优势 （比如一个使用了多个线程的计算密集型程序只会在一个单 CPU 上面运行）

GIL 只会影响到那些严重依赖 CPU 的程序（比如计算型的）

如果你的程序大部分只会设计到 I/O，比如网络交互，那么使用多线程就很合适， 因为它们大部分时间都在等待。实际上，你完全可以放心的创建几千个 Python 线程， 现代操作系统运行这么多线程没有任何压力，可直接用

而对于计算密集型任务，Python 下比较好的并行方式是使用多进程，这样可以非常有效的使用 CPU 资源。当然同一时间执行的进程数量取决你电脑的CPU核心数

## multiprocessing.Process 对象

multiprocessing.Process 对象是对进程的抽象，比如：

``` python
# -*- encoding: utf-8 -*-

from multiprocessing import Process
import os

def get_process(info):
    print info
    # *nix系统才有 getpid 及 getppid 方法
    print 'Process ID:', os.getpid()
    print 'Parent process ID:', os.getppid()

def func(name):
    get_process('In func:')
    print "Hello,", name

if __name__ == "__main__":
    get_process('In main:')
    p = Process(target=func, args=('marsloo',))
    # 开始子进程
    p.start()
    # 等待子进程结束
    p.join()
```

Process 对象的初始化参数为 `Process(group=None, target=None, name=None, args=(), kwargs={})`，其中 group 参数必须为 None（为了与 threading.Thread 的兼容），target 指向可调用对象（该对象在新的子进程中运行），name 是为该子进程命的名字（默认是 Process-1, Process-2, … 这样），args 是被调用对象的位置参数的元组列表，kwargs 是被调用对象的关键字参数。

子进程终结时会通知父进程并清空自己所占据的内存，在内核里留下退出信息(exit code，如果顺利运行，为0；如果有错误或异常状况，为大于零的整数)。父进程得知子进程终结后，需要对子进程使用 wait 系统调用，wait 函数会从内核中取出子进程的退出信息，并清空该信息在内核中占据的空间。

如果父进程早于子进程终结，子进程变成孤儿进程，孤儿进程会被过继给 init 进程，init 进程就成了该子进程的父进程，由 init 进程负责该子进程终结时调用 wait 函数。如果父进程不对子进程调用 wait 函数，子进程成为僵尸进程。僵尸进程积累时，会消耗大量内存空间。

如果在父进程中不调用 p.join 方法，则主进程与父进程并行工作：

``` python
from multiprocessing import Process
import time

def func():
    print "Child process start, %s" % time.ctime()
    time.sleep(2)
    print "Child process end, %s" % time.ctime()


if __name__ == "__main__":
    print "Parent process start, %s" % time.ctime()
    p = Process(target=func)
    p.start()
    # p.join()
    time.sleep(1)
    print "Parent process end, %s" % time.ctime()
```

上述代码的输出为：

``` text
Parent process start, Tue Jul 24 13:54:11 2018
Child process start, Tue Jul 24 13:54:11 2018
Parent process end, Tue Jul 24 13:54:12 2018
Child process end, Tue Jul 24 13:54:13 2018
```

如果开启了p.join的调用，输出如下：

``` text
Parent process start, Tue Jul 24 13:55:00 2018
Child process start, Tue Jul 24 13:55:00 2018
Child process end, Tue Jul 24 13:55:02 2018
Parent process end, Tue Jul 24 13:55:03 2018
```

另外一种情况是将子进程设置为 **守护进程**，则父进程在退出时不会关注子进程是否结束而直接退出：

``` python
from multiprocessing import Process
import time

def func():
    print "Child process start, %s" % time.ctime()
    time.sleep(2)
    print "Child process end, %s" % time.ctime()


if __name__ == "__main__":
    print "Parent process start, %s" % time.ctime()
    p = Process(target=func)
    # 守护进程一定要在start方法调用之前设置
    p.daemon = True
    p.start()
    # p.join()
    time.sleep(1)
    print "Parent process end, %s" % time.ctime()
```

上述代码输出为：

``` text
Parent process start, Tue Jul 24 14:00:19 2018
Child process start, Tue Jul 24 14:00:19 2018
Parent process end, Tue Jul 24 14:00:20 2018
Child process end, Tue Jul 24 14:00:21 2018
```

如果开启主进程对 join 方法的调用，主进程还是会等待守护子进程结束：

``` text
Parent process start, Tue Jul 24 14:06:24 2018
Child process start, Tue Jul 24 14:06:24 2018
Child process end, Tue Jul 24 14:06:26 2018
Parent process end, Tue Jul 24 14:06:27 2018
```

## 进程间通信

Interprocess communication，简称IPC

### Queue

使用 Queue 对象可以实现进程间通信，并且 Queue 对象是线程及进程安全的：

``` python
from multiprocessing import Queue, Process

def func(q):
    q.put([1, 'str', None])

if __name__ == "__main__":
    q = Queue()
    p = Process(target=func, args=(q,))
    p.start()
    p.join()
    print q.get()
```

如果声明了 `q = Queue(n)` 的 Queue 对象，则该对象的容量为 n

### Pipe

Pipe 对象返回的元组分别代表管道的两端，管道默认是全双工，两端都支持 send 和 recv 方法，两个进程分别操作管道两端时不会有冲突，两个进程对管道一端同时读写时可能会有冲突：

``` python
from multiprocessing import Pipe, Process

def func(p):
    p.send([1, 'str', None])
    p.close()

if __name__ == "__main__":
    parent_side, child_side = Pipe()
    p = Process(target=func, args=(child_side,))
    p.start()
    print parent_side.recv()
    p.join()
```

如果声明了 `p = Pipe(duplex=False)` 的单向管道，则 `p[0]` 只负责接受消息，`p[1]` 只负责发送消息

### 防止访问冲突与共享状态

比如多进程向 `stdout` 打印时，为了防止屏幕内容混乱可以加锁处理：

``` python
from multiprocessing import Lock, Process

def lock_func(l, number):
    l.acquire()
    print "Number is: %d" % number
    l.release()

if __name__ == "__main__":
    l = Lock()
    for number in range(10):
        Process(target=lock_func, args=(l, number,)).start()
```

并发编程中，应该尽量避免进程或线程间共享状态。在多线程中，共享资源可以使用全局变量或者传递参数。在多进程中，由于每个进程有自己独立的内存空间，以上方法并不合适，比如：

``` python
from multiprocessing import Process

a = [2]

def func(i):
    global a
    a[i] += 1
    print "a[0] in child prcocess:", a[0]

if __name__ == "__main__":
    p = Process(target=func, args=(0, ))
    p.start()
    p.join()
    print "a[0] in parent process:", a[0]
```

上述代码的运行结果为：

``` text
a[0] in child prcocess: 3
a[0] in parent process: 2
```

在多进程间共享状态可以使用共享内存或服务进程。需要注意的是，使用共享内存和服务进程需要加锁，否则多进程同时读写数据时会发生冲突

### 共享内存

在进程间共享状态可以使用 multiprocessing.Value 和 multiprocessing.Array 这样特殊的共享内存对象：

``` python
from multiprocessing import Process, Value, Array

def func(n, a):
    n.value = 3.1415926
    for i in range(len(a)):
        a[i] = -i

if __name__ == "__main__":
    # 'd'表示浮点型数据，'i'表示整数
    n = Value('d', 0.0)
    a = Array('i', range(10))
    p = Process(target=func, args=(n, a,))
    p.start()
    p.join()

    print n.value
    print a[:]
```

### 服务进程

multiprocessing.Manager 对象像是一个保存状态的代理，其他进程通过与代理的接口通信取得状态信息，服务进程支持更多的数据类型，使用起来比共享内存更灵活

``` python
from multiprocessing import Process, Manager

def func(d, l):
    d['1'] = 2
    d[2] = 'str'
    d[3.0] = None
    for i in range(len(l)):
        l[i] = -i

if __name__ == "__main__":
    m = Manager()
    l = m.list(range(10))
    d = m.dict()
    p = Process(target=func, args=(d, l,))
    p.start()
    p.join()

    print d
    print l
```

### 进程池

针对任务量巨大的场景，可以将进程放入进程池中，每个进程可以看做是一个 worker，比如：

``` python
from multiprocessing import Pool, TimeoutError
import os
import time

def func(x):
    return x * x

if __name__ == "__main__":
    p = Pool(processes=4)

    print p.map(func, range(10))

    for r in p.imap_unordered(func, range(10)):
        print r,
    print

    # 异步执行
    res = p.apply_async(func, (20,))
    print res.get(timeout=1)

    res = p.apply_async(os.getpid, ())
    print res.get(timeout=1)

    resutls = [p.apply_async(os.getpid, ()) for i in range(4)]
    print [r.get(timeout=1) for r in resutls]

    res = p.apply_async(time.sleep, (3,))
    try:
        print res.get(timeout=1)
    except TimeoutError:
        print "Timeout error!"
```

## 参考

1. [51.[Python]使用multiprocessing进行多进程编程](https://blog.csdn.net/a464057216/article/details/52735584)
2. [Python进阶：聊聊IO密集型任务、计算密集型任务，以及多线程、多进程](https://zhuanlan.zhihu.com/p/24283040)
3. [Python的全局锁(GIL)问题](https://blog.csdn.net/xsj_blog/article/details/70544103)