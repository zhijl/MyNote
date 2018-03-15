
### 拉取远程分支到本地

查看所有远程分支

``` shell
git branch -r
```

**方法一：**


``` shell
git checkout -b 本地分支名 origin/远程分支名
```

> 创建的本地分支会与远程分支建立映射关系

**方法二：**


``` shell
git fetch origin 远程分支名:本地分支名
```

> 创建的本地分支不会与远程分支建立映射关系