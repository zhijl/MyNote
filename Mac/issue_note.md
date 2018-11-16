# MacOS 使用过程中遇到的问题

### 安装 brew

```
==> This script will install:
/usr/local/bin/brew
/usr/local/share/doc/homebrew
/usr/local/share/man/man1/brew.1
/usr/local/share/zsh/site-functions/_brew
/usr/local/etc/bash_completion.d/brew
/usr/local/Homebrew

Press RETURN to continue or any other key to abort
==> Downloading and installing Homebrew...
HEAD is now at 57f7b8a8a Merge pull request #5181 from reitermarkus/upgrade-login-items
==> Tapping homebrew/core
Cloning into '/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core'...
remote: Enumerating objects: 4858, done.
remote: Counting objects: 100% (4858/4858), done.
remote: Compressing objects: 100% (4654/4654), done.
error: RPC failed; curl 56 LibreSSL SSL_read: SSL_ERROR_SYSCALL, errno 60
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed
Error: Failure while executing; `git clone https://github.com/Homebrew/homebrew-core /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1` exited with 128.
Error: Failure while executing; `/usr/local/bin/brew tap homebrew/core` exited with 1.
Failed during: /usr/local/bin/brew update --force
```

原因不太清楚，网上的解决方案是：

**终端走代理**

正好我电脑上配置了 Shadowsocks，所以直接将终端代理引向 Shadowsocks 即可：

```
export http_proxy=http://127.0.0.1:1087; export https_proxy=http://127.0.0.1:1087;
```