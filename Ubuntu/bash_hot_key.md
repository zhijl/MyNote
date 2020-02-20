# 掌握 Linux 中 Bash 命令的一些快捷方式

转自：https://www.cnblogs.com/WeiLian1024/p/11833715.html

## 控制屏幕

这些快捷方式用于控制终端屏幕输出：

- `Ctrl+L` –清除屏幕（效果与“ 清除 ”命令相同）
- `Ctrl+S` –暂停所有命令输出到屏幕。如果执行了产生冗长且冗长的输出的命令，请使用此命令暂停在屏幕上向下滚动的输出
- `Ctrl+Q` –使用 Ctrl+S 暂停后，恢复输出到屏幕

## 在命令行上移动光标

下一个快捷方式用于在命令行中移动光标：

- `Ctrl+A` 或 Home 将光标移动到行首
- `Ctrl+E` 或 End 将光标移动到行尾
- `Ctrl+B` 或 Left Arrow 一次将光标移回一个字符
- `Ctrl+F` 或 Right Arrow 一次将光标向前移动一个字符
- `Ctrl+ Left Arrow` 或 Alt+B 或 Esc 然后 B 一次将光标移回一个单词
- `Ctrl+ Right Arrow` 或 Alt+C 或 Esc 然后 F 一次将光标向前移动一个单词

## 搜索Bash历史记录

以下快捷方式用于在bash历史记录中搜索命令：

- `Up arrow key` – 检索上一个命令。如果持续按下它，它将带您浏览历史记录中的多个命令，因此您可以找到所需的命令。使用向下箭头在历史记录中反向移动
- `Ctrl+P` 和 `Ctrl+N`–分别用于向上和向下箭头键的替代
- `Ctrl+R` –在 bash 历史记录中开始反向搜索，只需键入要在历史记录中查找的命令所独有的字符
- `Ctrl+S` –通过 bash 历史记录启动向前搜索
- `Ctrl+G` –在 bash 历史记录中退出反向或向前搜索

## 在命令行上删除文本

以下快捷方式用于在命令行上删除文本：

- `Ctrl+D` 或 Delete –删除或删除光标下方的字符
- `Ctrl+K` –将所有文本从光标移至行尾
- `Ctrl+X` 然后 Backspace –将所有文本从光标移至该行的开头

## 在命令行上转置文本或更改大小写

这些快捷方式将转置或更改命令行中字母或单词的大小写：

- `Ctrl+T` –将光标前的字符与光标下的字符进行转置
- Esc 然后 T –紧接在光标之前（或下方）移开两个单词
- Esc 然后 U –将文本从光标到单词的结尾转换为大写
- Esc 然后 L –将文本从光标到单词的结尾转换为小写
- Esc 然后 C –将光标下方的字母（或下一个单词的第一个字母）更改为大写，其余单词保持不变

## 在 Linux 中使用进程

以下快捷方式可帮助控制正在运行的 Linux 进程

- `Ctrl+Z` –暂停当前的前台进程。这会将 SIGTSTP 信号发送到该进程。你可以稍后使用 `fg process_name（或％bgprocess_number，如％1，％2等）` 使该进程回到前台
- `Ctrl+C` –通过向其发送 SIGINT 信号来中断当前的前台进程。默认行为是正常终止进程，但是进程可以接受或忽略它
- `Ctrl+D` –退出bash shell（与运行exit命令相同）

## Bash Bang（！）的命令

在本文的最后一部分，我们将解释一些关于（！）的操作：

- `!!` –执行最后一条命令
- `!top` –执行以 'top' 开头的最新命令
- `!top:p` –显示 ！top 将运行的命令（也将其添加为历史记录中的最新命令）
- `!$` –执行上一个命令的最后一个单词（与 Alt +。相同，例：如果最后一个命令是' cat tecmint.txt '，那么！$将尝试运行' tecmint.txt '）
- `!$:p` –显示 ！$ 将执行的单词
- `!*` –显示上一个命令的最后一个单词
- `!*:p` –显示 ！* 替代的最后一个单词