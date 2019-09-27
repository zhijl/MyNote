# 编译新版本 VIM

Ubuntu 16.04 apt 源中的 VIM 版本不是很新，安装新版 VIM 需要从源码编译

``` sh
git clone https://github.com/vim/vim.git
cd vim
git pull

# 配置python支持，否则在

./configure --enable-multibyte --enable-pythoninterp=yes

编译：

cd src
make distclean # if you build Vim before
make
sudo make install
```

``` sh
vim --version
VIM - Vi IMproved 8.1 (2018 May 18, compiled Sep 27 2019 14:11:41)
```
