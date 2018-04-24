
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

### 删除分支

查看所有分支

``` shell
git branch -a
```

删除远程分支

``` shell
git push origin --delete branch_name
```

删除本地分支

``` shell
git branch -d branch_name
```

### Git 拉取远程分支到本地

``` shell
git branch origin remote_branch_nameA:loacl_branch_nameA
```