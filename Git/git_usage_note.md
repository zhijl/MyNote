# 常用的 git 操作

## 标签

``` shell
git tag # 显示所有标签号
git tag -l xxx* # 匹配查询标签号
git show xxx # 查看标签号 xxx 的说明信息
git tag xxx # xxx 为标签号
git tag xxx -m xxxx # 打标签并附上说明信息（annotated）
git describe --tags `git rev-list --tags --max-count=1` # 获取最新的标签号
git tag -a xxx xxxxx # 后续加标签，xxx 为标签号，xxxxx 为 commit 号
git log --pretty=oneline  # 按一行一行的形式显示历史 commit 信息
```

``` shell
git push origin xxx # 把标签 xxx 同步原远程
git push origin --tags # 推送本地所有未同步的标签
```

``` shell
git tag -d xxx # 删除标签(本地的)
git push origin :refs/tags/xxx # 把删除状态同步到远程
```

``` shell
git checkout xxx # 切入某个标签
```

## 分支

``` shell
git branch # 列出所有分支
git branch xxx  # 从当前 commit 出来一个 xxx 分支
git checkout xxx # 切入该分支
git checkout -b xxx # 创建并切入一个新分支 xxx
git branch -d xxx # 删除分支
```

``` sh
git merge xxx  # 把 xxx 分支里的东西合并到当前分支中
```

**conflict 问题**

同一个文件的同一个地方在两个分支中都被不一致地修改并 commit 了，之后 merge 时会出现 conflict 问题

解决：

``` sh
git diff # 查看冲突
## 1. 手动修改两个分支中的冲突点 xxxx
## 2. 再次 add 修改后的文件，再提交
```
