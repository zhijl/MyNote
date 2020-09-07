# 安装 MacOS 平台的 TeX 工具

我使用 VS Code 作为编辑器，这样只需要下载和配置 TeX 的宏包和编译器即可

需提前准备：

- brew
- VS Code

### 安装和配置

- 安装 MacTex 或者 BasicTex

MacTex 相比 BasicTex 东西更多更全，缺点是太大，大概 3.x GB，而 BasicTex 据说只有 70+MB。安装 BasicTex 后，使用中遇到有缺失的东西再安装也可以。此处为了省事，直接安装了 MacTex。

``` sh
brew update
brew cask install mactex # (或者) brew cask install basictex
```

> 提示：brew 可以替换为清华源来加速下载 [https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

- VS Code 插件 -> LaTeX Workshop 安装

- 配置 LaTeX Workshop，在 VS Code 中 `command + ,` 打开设置面板，打开 settings.json 对其中的 `latex-workshop.latex.tools` 和 `latex-workshop.latex.recipes` 添加如下内容

``` json
    "latex-workshop.latex.tools": [
        {
            "name": "xelatex",
            "command": "xelatex",
            "args": [
                "-synctex=1",
                "-interaction=nonstopmode",
                "-file-line-error",
                "-pdf",
                "%DOC%"
            ]
        },
        ...
    ],
    "latex-workshop.latex.recipes": [
        {
            "name": "xelatex",
            "tools": [
                "xelatex"
            ]
        },
        ...
    ]
```

至此，安装和配置完成，但是在对一个 tex 进行编辑并保存时，出现了一些报错，下面对这些问题进行了记录和解决。

### 问题记录

**1. Can not found latexindent in PATH**

但是查看 Terminal 中，`which latexindent` 是可以被找到的，后来重启电脑后解决了这个问题。初步怀疑安装 mactex 后，默认将 bin 目录即 `/Library/TeX/texbin` 添加到了系统某个配置文件中，不同于 `.bashrc` 或 `.zshrc` ，它需要重启，重新载入配置文件后再能激活这个路径。目前暂不深究这个问题，反正能找到了~

**2. Log::Log4perl 等 perl 库未安装**

> 注意：先别急着按下面流程 install！！！先阅读一遍下面的安装流程！！！

报错如下：

``` text
stderr: Can't locate Log/Log4perl.pm in @INC (you may need to install the Log::Log4perl module) (@INC contains: /usr/local/texlive/2020/texmf-dist/scripts/latexindent /Library/Perl/5.18/darwin-thread-multi-2level /Library/Perl/5.18 /Network/Library/Perl/5.18/darwin-thread-multi-2level /Network/Library/Perl/5.18 /Library/Perl/Updates/5.18.4 /System/Library/Perl/5.18/darwin-thread-multi-2level /System/Library/Perl/5.18 /System/Library/Perl/Extras/5.18/darwin-thread-multi-2level /System/Library/Perl/Extras/5.18 .) at /usr/local/texlive/2020/texmf-dist/scripts/latexindent/LatexIndent/LogFile.pm line 22.
BEGIN failed--compilation aborted at /usr/local/texlive/2020/texmf-dist/scripts/latexindent/LatexIndent/LogFile.pm line 22.
...
```

报错日志提示 log4perl 这个库没安装，可以使用 `cpan` 工具安装缺失的 perl 依赖包，后来发现缺失了不少库，安装如下：

``` sh
sudo cpan install Log::Log4perl
sudo cpan install Log::Dispatch::File
sudo cpan install YAML::Tiny
sudo cpan install File::HomeDir
sudo cpan install Unicode::GCString
```

在安装 `File::HomeDir` 的时候报错：

``` text
Running make install
  make test had returned bad status, won't install without force
```

提示告知要加 `force` 才能安装成功，但是实际上是：

```
Checksum for /Users/zhi/.cpan/sources/authors/id/E/ET/ETHER/Mac-SystemDirectory-0.13.tar.gz ok

  CPAN.pm: Building E/ET/ETHER/Mac-SystemDirectory-0.13.tar.gz

HASCOMPILERMrlq/TESTcmrC.c:2:10: fatal error: 'EXTERN.h' file not found
#include "EXTERN.h"
         ^~~~~~~~~~
1 error generated.
```

这个地方导致的问题，问题不好解决了...

---

关于 "EXTERN.h" 的缺失，可以尝试 [MAC 下解决 latexindent 不能用的问题](https://haoyu.love/blog811.html) 中提到的方法，即

``` sh
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.15.pkg
或者
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```

但是我没有再继续尝试走这条路线，不过这篇文中也提到一些不错的点值得注意：

1. 将 perl 的 cpan 下载的依赖安装在用户空间里面，而不是默认的系统空间（这样可以不用 sudo 也能装）
2. 使用 cpanm 安装相关依赖

```
PERL_MM_OPT="INSTALL_BASE=$HOME/.perl5" cpan local::lib

brew install cpanm
cpanm --local-lib=~/.perl5 local::lib && eval $(perl -I ~/.perl5/lib/perl5/ -Mlocal::lib)
cpanm Log::Log4perl Log::Dispatch::File YAML::Tiny File::HomeDir Unicode::GCString
```

---

我决定尝试另一种方法：

即使用先使用 brew 安装的 perl

``` sh
brew install perl
```

根据 brew 打出的提示，我按照提示配置了 perl

``` sh
PERL_MM_OPT="INSTALL_BASE=$HOME/perl5" cpan local::lib
echo 'eval "$(perl -I$HOME/perl5/lib/perl5 -Mlocal::lib=$HOME/perl5)"' >> ~/.zshrc
```

然后安装缺失依赖：

``` sh
cpan Log::Log4perl
cpan Log::Dispatch::File
cpan YAML::Tiny
cpan Mac::SystemDirectory
cpan File::HomeDir
cpan Unicode::GCString
```

这里在安装 `File::HomeDir` 时能正常安装，但是安装 `Unicode::GCString` 报错了...

报错关键信息如下：

``` text
t/13flowedsp.t ........... Can't load '/Users/zhi/.cpan/build/Unicode-LineBreak-2019.001-8/blib/arch/auto/Unicode/LineBreak/LineBreak.bundle' for module Unicode::LineBreak: dlopen(/Users/zhi/.cpan/build/Unicode-LineBreak-2019.001-8/blib/arch/auto/Unicode/LineBreak/LineBreak.bundle, 2): Symbol not found: _linebreak_format_NEWLINE
  Referenced from: /Users/zhi/.cpan/build/Unicode-LineBreak-2019.001-8/blib/arch/auto/Unicode/LineBreak/LineBreak.bundle
  Expected in: flat namespace
```

即 Unicode::LineBreak 这个依赖出问题了，主要问题在于 `LineBreak.bundle` 中有未定义的符号，而实际上 `LineBreak.bundle` 为一个动态库，为了了解这个动态库为什么有缺失的依赖，我找到了 cpan 的 build 目录，即 `~/.cpan/build/Unicode-LineBreak-2019.001-8`，`cpan Unicode::LineBreak` 在安装这个 perl 包的时候，其实就是把它的源码从服务器拉取下来，解压后按照既定的编译流程进行编译，这里通过 `README` 文件，可以知道其编译流程为 

``` sh
perl Makefile.PL
make
make test
make install
```

通过阅读 `Makefile`，我找到了 `LineBreak.bundle` 的生成语句，初步怀疑是 `$(MYEXTLIB)` （即 `sombok/libsombok.a`）这个静态库生成有 bug，后来我将生成这个静态库的所有 `*.o` 直接替代 `sombok/libsombok.a` 添加在生成 `LineBreak.bundle` 的链接缓解中。即将原来的

```
 $(INST_DYNAMIC) : $(OBJECT) $(MYEXTLIB) $(INST_ARCHAUTODIR)$(DFSEP).exists $(EXPORT_LIST) $(PERL_ARCHIVEDEP) $(PERL_ARCHIVE_AFTER) $(INST_DYNAMIC_DEP)
   $(RM_F) $@
   $(LD)  $(LDDLFLAGS)  $(LDFROM) $(OTHERLDFLAGS) -o $@ $(MYEXTLIB) \
     $(PERL_ARCHIVE) $(LDLOADLIBS) $(PERL_ARCHIVE_AFTER) $(EXPORT_LIST) \
     $(INST_DYNAMIC_FIX)
   $(CHMOD) $(PERM_RWX) $@
```

修改为：

```
 $(INST_DYNAMIC) : $(OBJECT) $(MYEXTLIB) $(INST_ARCHAUTODIR)$(DFSEP).exists $(EXPORT_LIST) $(PERL_ARCHIVEDEP) $(PERL_ARCHIVE_AFTER) $(INST_DYNAMIC_DEP)
   $(RM_F) $@
   $(LD)  $(LDDLFLAGS)  $(LDFROM) $(OTHERLDFLAGS) sombok/lib/8.0.0.o sombok/lib/break.o sombok/lib/charprop.o sombok/lib/gcstring.o sombok/lib/southeastasian.o sombok/lib/utf8.o sombok/lib/utils.o -o $@ \     $(PERL_ARCHIVE) $(LDLOADLIBS) $(PERL_ARCHIVE_AFTER) $(EXPORT_LIST) \
     $(INST_DYNAMIC_FIX)
   $(CHMOD) $(PERM_RWX) $@
```

重新 make 和 make install 后，再在终端执行 latexindent，便可以成功运行！至此在 Mac 上安装 TeX 的过程就结束了。

---

另外还有一些值得注意的点：

- `cpan` 没有卸载操作，一般卸载方式是进入到 build 的源码目录下，执行 make uninstall；另外也有人提到使用 `cpanm --uninstall Module::Name` 的方式
- `mactex` 没有安装在 brew 的 `Cellar` 目录下，但是安装完之后，终端可以 which 到 mactex 的 bin 目录下的工具，原因在于 mactex 在安装的时候创建了一个文件 `/etc/paths.d/TeX`，这个文件中指定了其 bin 目录。在 brew 安装完 mactex 后，VS Code 中的LaTeX Workshop 没有找到 bin 目录下的一些工具如 latext、latexindent，但是重启 mac 系统后，就找到了，可能也是这个原因 [how is /Library/TeX/texbin added to $PATH](https://latex.org/forum/viewtopic.php?t=26726)

参考：[https://tex.stackexchange.com/questions/445521/latexindent-cant-locate-log-log4perl-pm-in-inc-you-may-need-to-install-the-l](https://tex.stackexchange.com/questions/445521/latexindent-cant-locate-log-log4perl-pm-in-inc-you-may-need-to-install-the-l)