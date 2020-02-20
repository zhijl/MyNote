### 获取最新的 tag

``` sh
git describe --tags `git rev-list --tags --max-count=1`
```

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

### clone 仓库时，只 clone 最近一次 commit

``` shell
git clone --depth 1 url
```

### clone 某一分支

``` sh
git clone -b branch_name url
```

**如果要推送空的目录到远程服务器，可以在该空目录下创建一个.gitkeep文件即可**