# mmcv 项目源码剖析

计划对 mmcv 项目的源码进行剖析和注释，并在这个过程中补充一些 Python 的笔记

``` text
mmcv/
├── docs
│   ├── api.rst
│   ├── cnn.md
│   ├── conf.py
│   ├── image.md
│   ├── index.rst
│   ├── io.md
│   ├── make.bat
│   ├── Makefile
│   ├── runner.md
│   ├── _static
│   │   ├── parallel_progress.gif
│   │   └── progress.gif
│   ├── utils.md
│   ├── video.md
│   └── visualization.md
├── examples
│   ├── config_cifar10.py
│   ├── dist_train_cifar10.sh
│   ├── resnet_cifar.py
│   └── train_cifar10.py
├── LICENSE
├── mmcv
│   ├── arraymisc
│   │   ├── __init__.py
│   │   └── quantization.py
│   ├── cnn
│   │   ├── alexnet.py
│   │   ├── __init__.py
│   │   ├── resnet.py
│   │   ├── vgg.py
│   │   └── weight_init.py
│   ├── fileio
│   │   ├── handlers
│   │   │   ├── base.py
│   │   │   ├── __init__.py
│   │   │   ├── json_handler.py
│   │   │   ├── pickle_handler.py
│   │   │   └── yaml_handler.py
│   │   ├── __init__.py
│   │   ├── io.py
│   │   └── parse.py
│   ├── image
│   │   ├── __init__.py
│   │   ├── io.py
│   │   └── transforms
│   │       ├── colorspace.py
│   │       ├── geometry.py
│   │       ├── __init__.py
│   │       ├── normalize.py
│   │       └── resize.py
│   ├── __init__.py
│   ├── opencv_info.py
│   ├── parallel
│   │   ├── collate.py
│   │   ├── data_container.py
│   │   ├── data_parallel.py
│   │   ├── distributed.py
│   │   ├── _functions.py
│   │   ├── __init__.py
│   │   └── scatter_gather.py
│   ├── runner
│   │   ├── checkpoint.py
│   │   ├── hooks
│   │   │   ├── checkpoint.py
│   │   │   ├── closure.py
│   │   │   ├── hook.py
│   │   │   ├── __init__.py
│   │   │   ├── iter_timer.py
│   │   │   ├── logger
│   │   │   │   ├── base.py
│   │   │   │   ├── __init__.py
│   │   │   │   ├── pavi.py
│   │   │   │   ├── tensorboard.py
│   │   │   │   └── text.py
│   │   │   ├── lr_updater.py
│   │   │   ├── memory.py
│   │   │   ├── optimizer.py
│   │   │   └── sampler_seed.py
│   │   ├── __init__.py
│   │   ├── log_buffer.py
│   │   ├── parallel_test.py
│   │   ├── priority.py
│   │   ├── runner.py
│   │   └── utils.py
│   ├── utils
│   │   ├── config.py
│   │   ├── __init__.py
│   │   ├── misc.py
│   │   ├── path.py
│   │   ├── progressbar.py
│   │   └── timer.py
│   ├── version.py
│   ├── video
│   │   ├── __init__.py
│   │   ├── io.py
│   │   ├── optflow.py
│   │   └── processing.py
│   └── visualization
│       ├── color.py
│       ├── image.py
│       ├── __init__.py
│       └── optflow.py
├── README.rst
├── requirements.txt
├── setup.cfg
├── setup.py
└── tests
    ├── data
    │   ├── color.jpg
    │   ├── config
    │   │   ├── a.b.py
    │   │   ├── a.py
    │   │   ├── b.json
    │   │   └── c.yaml
    │   ├── filelist.txt
    │   ├── for_scan
    │   │   ├── 1.json
    │   │   ├── 1.txt
    │   │   ├── 2.json
    │   │   ├── 2.txt
    │   │   ├── a.bin
    │   │   └── sub
    │   │       ├── 1.json
    │   │       └── 1.txt
    │   ├── grayscale.jpg
    │   ├── mapping.txt
    │   ├── optflow_concat0.jpg
    │   ├── optflow_concat1.jpg
    │   ├── optflow.flo
    │   ├── patches
    │   │   ├── 0.npy
    │   │   ├── 1.npy
    │   │   ├── 2.npy
    │   │   ├── 3.npy
    │   │   ├── 4.npy
    │   │   ├── pad0_0.npy
    │   │   ├── pad0_1.npy
    │   │   ├── pad0_2.npy
    │   │   ├── pad0_3.npy
    │   │   ├── pad0_4.npy
    │   │   ├── pad_0.npy
    │   │   ├── pad_1.npy
    │   │   ├── pad_2.npy
    │   │   ├── pad_3.npy
    │   │   ├── pad_4.npy
    │   │   ├── scale_0.npy
    │   │   ├── scale_1.npy
    │   │   ├── scale_2.npy
    │   │   ├── scale_3.npy
    │   │   └── scale_4.npy
    │   └── test.mp4
    ├── test_arraymisc.py
    ├── test_config.py
    ├── test_fileio.py
    ├── test_image.py
    ├── test_misc.py
    ├── test_optflow.py
    ├── test_path.py
    ├── test_progressbar.py
    ├── test_timer.py
    ├── test_video.py
    └── test_visualization.py

23 directories, 140 files
```

## image module

**__init__.py**

__init__.py 文件的作用是将文件夹变为一个Python模块,Python 中的每个模块的包中，都有__init__.py 文件

通常__init__.py 文件为空，但是我们还可以为它增加其他的功能。我们在导入一个包时，实际上是导入了它的__init__.py文件。这样我们可以在__init__.py文件中批量导入我们所需要的模块，而不再需要一个一个的导入

`__init__.py` 中的重要变量 `__all__`

**__all__ 变量**

python 模块中的 `__all__` 属性，可用于模块导入时限制，如：

``` py
from module import *
```

此时被导入模块若定义了 `__all__` 属性，则只有 `__all__` 内指定的属性、方法、类可被导入

若没定义，则导入模块内的所有公有属性，方法和类

这时就会把注册在 __init__.py 文件中 __all__ 列表中的模块和包导入到当前文件中来

可以了解到，__init__.py 主要控制包的导入行为。要想清楚理解 __init__.py 文件的作用，还需要详细了解一下 import 语句引用机制：

可以被 import 语句导入的对象是以下类型：

- 模块文件（ .py 文件）
- C 或 C++ 扩展（已编译为共享库或 DLL 文件）
- 包（包含多个模块）
- 内建模块（使用 C 编写并已链接到 Python 解释器中）
- 当导入模块时，解释器按照 sys.path 列表中的目录顺序来查找导入文件。

``` py
import sys
>>> print(sys.path)

# Linux:
['', '/usr/local/lib/python3.4',
 '/usr/local/lib/python3.4/plat-sunos5',
 '/usr/local/lib/python3.4/lib-tk',
 '/usr/local/lib/python3.4/lib-dynload',
 '/usr/local/lib/python3.4/site-packages']   # 第一个空串表示当前目录
```

**关于.pyc 文件 与 .pyo 文件**

.py 文件的汇编，只有在 import 语句执行时进行，当 .py 文件第一次被导入时，它会被汇编为字节代码，并将字节码写入同名的 .pyc 文件中。后来每次导入操作都会直接执行 .pyc 文件（当 .py 文件的修改时间发生改变，这样会生成新的 .pyc 文件），在解释器使用 -O 选项时，将使用同名的 .pyo 文件，这个文件去掉了断言（assert）、断行号以及其他调试信息，体积更小，运行更快。（使用 -OO 选项，生成的.pyo 文件会忽略文档信息）

**导入模块**

模块通常为单独的 .py 文件，可以用 import 直接引用，可以作为模块的文件类型有 .py、.pyo、.pyc、.pyd、.so、.dll

在导入模块时，解释器做以下工作：

已导入模块的名称创建新的命名空间，通过该命名空间就可以访问导入模块的属性和方法

在新创建的命名空间中执行源代码文件

创建一个名为源代码文件的对象，该对象引用模块的名字空间，这样就可以通过这个对象访问模块中的函数及变量

import 语句可以在程序的任何位置使用，你可以在程序中多次导入同一个模块，但模块中的代码仅仅在该模块被首次导入时执行。后面的import语句只是简单的创建一个到模块名字空间的引用而已

sys.modules 字典中保存着所有被导入模块的模块名到模块对象的映射

**导入包**

多个相关联的模块组成一个包，以便于维护和使用，同时能有限的避免命名空间的冲突。一般来说，包的结构可以是这样的：

``` text
package
  |- subpackage1
      |- __init__.py
      |- a.py
  |- subpackage2
      |- __init__.py
      |- b.py
```

有以下几种导入方式：

``` py
import subpackage1.a # 将模块subpackage.a导入全局命名空间，例如访问a中属性时用subpackage1.a.attr
from subpackage1 import a #　将模块a导入全局命名空间，例如访问a中属性时用a.attr_a
from subpackage.a import attr_a # 将模块a的属性直接导入到命名空间中，例如访问a中属性时直接用attr_a
```

使用 from 语句可以把模块直接导入当前命名空间，from 语句并不引用导入对象的命名空间，而是将被导入对象直接引入当前命名空间

> https://www.cnblogs.com/Lands-ljk/p/5880483.html

#### `mmcv/image/__init__.py`

``` py
from .io import imread, imwrite, imfrombytes
from .transforms import (bgr2gray, gray2bgr, bgr2rgb, rgb2bgr, bgr2hsv,
                         hsv2bgr, iminvert, imflip, imrotate, imcrop, impad,
                         impad_to_multiple, imnormalize, imdenormalize,
                         imresize, imresize_like, imrescale)

__all__ = [
    'imread', 'imwrite', 'imfrombytes', 'bgr2gray', 'gray2bgr', 'bgr2rgb',
    'rgb2bgr', 'bgr2hsv', 'hsv2bgr', 'iminvert', 'imflip', 'imrotate',
    'imcrop', 'impad', 'impad_to_multiple', 'imnormalize', 'imdenormalize',
    'imresize', 'imresize_like', 'imrescale'
]
```

from 后面的 `.io` 中的点表示当前路径下，`io` 是一个 python 文件，即 `io.py`，先解析这个文件中的内容

``` py
import os.path as osp

import cv2
import numpy as np

from mmcv.utils import is_str, check_file_exist, mkdir_or_exist
from mmcv.opencv_info import USE_OPENCV2

# OpenCV 版本兼容
if not USE_OPENCV2:
    from cv2 import IMREAD_COLOR, IMREAD_GRAYSCALE, IMREAD_UNCHANGED
else:
    from cv2 import CV_LOAD_IMAGE_COLOR as IMREAD_COLOR    # 老版本 OpenCV 宏定义
    from cv2 import CV_LOAD_IMAGE_GRAYSCALE as IMREAD_GRAYSCALE
    from cv2 import CV_LOAD_IMAGE_UNCHANGED as IMREAD_UNCHANGED  # 包含 alpha 通道

imread_flags = {
    'color': IMREAD_COLOR,
    'grayscale': IMREAD_GRAYSCALE,
    'unchanged': IMREAD_UNCHANGED
}

# 读取图像，这里的 flag 也可以是数字
def imread(img_or_path, flag='color'):
    """Read an image.

    Args:
        img_or_path (ndarray or str): Either a numpy array or image path.
            If it is a numpy array (loaded image), then it will be returned
            as is.
        flag (str): Flags specifying the color type of a loaded image,
            candidates are `color`, `grayscale` and `unchanged`.

    Returns:
        ndarray: Loaded image array.
    """
    if isinstance(img_or_path, np.ndarray):
        return img_or_path
    elif is_str(img_or_path):
        flag = imread_flags[flag] if is_str(flag) else flag
        check_file_exist(img_or_path,
                         'img file does not exist: {}'.format(img_or_path))
        return cv2.imread(img_or_path, flag)
    else:
        raise TypeError('"img" must be a numpy array or a filename')

# 从文件的二进制流数组中解码得到图像矩阵
def imfrombytes(content, flag='color'):
    """Read an image from bytes.

    Args:
        content (bytes): Image bytes got from files or other streams.
        flag (str): Same as :func:`imread`.

    Returns:
        ndarray: Loaded image array.
    """
    # 输入含有 __buffer__ 的对象，例如 b'23423' 二进制字符串，将该输入按照指定的类型分解到数组中
    img_np = np.frombuffer(content, np.uint8)
    flag = imread_flags[flag] if is_str(flag) else flag  # flag 非法时 imread_flags[flag] 会跑出异常
    img = cv2.imdecode(img_np, flag)
    return img


def imwrite(img, file_path, params=None, auto_mkdir=True):
    """Write image to file

    Args:
        img (ndarray): Image array to be written.
        file_path (str): Image file path.
        params (None or list): Same as opencv's :func:`imwrite` interface.
        auto_mkdir (bool): If the parent folder of `file_path` does not exist,
            whether to create it automatically.

    Returns:
        bool: Successful or not.
    """
    if auto_mkdir:
        dir_name = osp.abspath(osp.dirname(file_path))
        mkdir_or_exist(dir_name)
    return cv2.imwrite(file_path, img, params)
```

**Python os 模块**

``` py
import os, os.path

os.path.pardir                  # 返回相对与当前源文件所在的上一级目录
os.path.abspath("test.txt")     # 返回当前文件所在全路径，结果返回 全路径 + /test.txt
os.path.dirname("/234/234234//234/234")     # 返回当前文件所在路径，即将最后一个 / 及后的字符串去掉
os.path.join(os.path.dirname("__file__"), os.path.pardir) # 以路径的模式连接两个字符串，并返回路径
os.getcwd()     # 返回 python 脚本工作的路径（全路径）
os.listdir()    # 返回指定目录下的所有文件和目录名
os.remove("test.txt")       # 删除一个文件
os.path.isfile("test.txt")  # 判断是否是文件
os.path.isabs(str)    # 判断是否是全路径
os.path.isdir(str)    # 判断是否是路径
os.path.exists(str)   # 路径是否存在
os.path.splitext()    # 分离扩展名
os.path.dirname()     # 获取路径名
os.path.basename()    # 获取文件名
os.system()           # 运行shell命令
os.getenv() 与 os.putenv()  # 读取和设置环境变量
os.linesep            # 给出当前平台使用的行终止符: Windows使用 ’\r\n’，Linux 使用 ’\n’ 而 Mac 使用 ’\r’
os.name               # 指示你正在使用的平台：对于 Windows，它是’nt’，而对于 Linux/Unix 用户，它是’posix’
os.rename（old， new） # 重命名
os.makedirs（r"c：\python\test"）     # 创建多级目录
os.mkdir("test")                     # 创建单个目录
os.makedirs("/test/test/tep")        # 递归创建单个目录
os.stat(file)                        # 获取文件属性
os.chmod(file)                       # 修改文件权限与时间戳
os.exit()                            # 终止当前进程
os.path.getsize(filename)            # 获取文件大小
os.path.expanduser(path)             # 把 path 中包含的 "~" 和 "~user" 转换成用户目录
```

os 库常用的导入方式 `import os, os.path as osp`

> 原文：https://blog.csdn.net/xiongchengluo1129/article/details/
> https://www.runoob.com/python/python-os-path.html


**mmcv/image/transforms/__init__.py**

``` py
from .colorspace import (bgr2gray, gray2bgr, bgr2rgb, rgb2bgr, bgr2hsv,
                         hsv2bgr, iminvert)
from .geometry import imflip, imrotate, imcrop, impad, impad_to_multiple
from .normalize import imnormalize, imdenormalize
from .resize import imresize, imresize_like, imrescale

__all__ = [
    'bgr2gray', 'gray2bgr', 'bgr2rgb', 'rgb2bgr', 'bgr2hsv', 'hsv2bgr',
    'iminvert', 'imflip', 'imrotate', 'imcrop', 'impad', 'impad_to_multiple',
    'imnormalize', 'imdenormalize', 'imresize', 'imresize_like', 'imrescale'
]
```

**mmcv/image/transforms/colorspace.py**

``` py
import cv2
import numpy as np

# 将一张图像的值进行翻转
def iminvert(img):
    """Invert (negate) an image
    Args:
        img (ndarray): Image to be inverted.

    Returns:
        ndarray: The inverted image.
    """
    return np.full_like(img, 255) - img  # full_like 返回与传入数组相同大小的数组，并用 255 进行填充


def bgr2gray(img, keepdim=False):
    """Convert a BGR image to grayscale image.

    Args:
        img (ndarray): The input image.
        keepdim (bool): If False (by default), then return the grayscale image
            with 2 dims, otherwise 3 dims.

    Returns:
        ndarray: The converted grayscale image.
    """
    out_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    if keepdim:
        out_img = out_img[..., None]  # 增加一个维度
    return out_img


def gray2bgr(img):
    """Convert a grayscale image to BGR image.

    Args:
        img (ndarray or str): The input image.

    Returns:
        ndarray: The converted BGR image.
    """
    img = img[..., None] if img.ndim == 2 else img
    out_img = cv2.cvtColor(img, cv2.COLOR_GRAY2BGR)
    return out_img


def convert_color_factory(src, dst):

    # getattr 获取对象内的一个属性，如果不存在会抛出异常
    code = getattr(cv2, 'COLOR_{}2{}'.format(src.upper(), dst.upper()))

    def convert_color(img):
        out_img = cv2.cvtColor(img, code)
        return out_img

    # 为内置的 convert_color 函数设置注释文档
    convert_color.__doc__ = """Convert a {0} image to {1} image.

    Args:
        img (ndarray or str): The input image.

    Returns:
        ndarray: The converted {1} image.
    """.format(src.upper(), dst.upper())

    return convert_color


bgr2rgb = convert_color_factory('bgr', 'rgb')

rgb2bgr = convert_color_factory('rgb', 'bgr')

bgr2hsv = convert_color_factory('bgr', 'hsv')

hsv2bgr = convert_color_factory('hsv', 'bgr')
```

**字符串格式化**

``` py
"{} {}".format("hello", "world")    # 不设置指定位置，按默认顺序
"{1} {0} {1}".format("hello", "world")  # 设置指定位置

print("site：{name}, url： {url}".format(name="abc", url="www.abc.com"))

# 通过字典设置参数
site = {"name": "abc", "url": "www.abc.com"}
print("site：{name}, url {url}".format(**site))

# 通过列表索引设置参数
my_list = ['abc', 'www.abc.com']
print("site：{0[0]}, url {0[1]}".format(my_list))  # "0" 是必须的

# 格式化
print("{:.2f}".format(3.1415926))  # 保留小数点后两位
print("{:+.2f}".format(3.1415926)) # 带符号保留小数点后两位

{:0>2d}   # 位数为 2，不足左边补零
{:x<4d}	  # 位数为 4，不足左边补 x

```



> https://www.runoob.com/python/att-string-format.html
