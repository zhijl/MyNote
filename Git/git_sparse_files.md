# Git 下载单个文件或文件夹

启用 Git 1.7 中支持的 `sparse checkout` 模式

``` sh
# 新建一个本地仓库
mkdir <repository>
cd <repository>
git init
git remote add -f origin <url>
git config core.sparsecheckout true
# 告诉 git 哪些文件需要 check-out
echo libs >> .git/info/sparse-checkout
echo apps/register.go >> .git/info/sparse-checkout
echo resource/css >> .git/info/sparse-checkout

git pull origin master
```
