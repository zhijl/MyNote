# 使用 Mac 的一些记录

## Bash 终端配置文件

``` text
.bash_profile
```

## Homebrew 完全卸载软件及其依赖

使用 `brew uninstall git` 卸载软件只会卸载软件本身而不会同时卸载其依赖包，使用以下命令可完全卸载，并且不会影响到其他软件

``` sh
brew tap beeftornado/rmtree
```

这一步更新时间可能比较长，请耐心等待。结束后执行如下命令即可完全卸载软件

``` sh
brew rmtree git
brew cleanup
```

摘自：https://blog.csdn.net/mandagod/article/details/89737634
