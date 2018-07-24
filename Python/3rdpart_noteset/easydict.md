# EasyDict

> 可以像访问属性一样访问dict里的变量

``` python
>>> from easydict import EasyDict as edict
>>> d = edict({'foo':3, 'bar':{'x':1, 'y':2}})
>>> d.foo
3
>>> d.bar.x
1

>>> d = edict(foo=3)
>>> d.foo
3
```

## 参考

1. https://www.zhihu.com/question/64834166/answer/276406439