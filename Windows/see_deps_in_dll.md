# 使用 VS 提供的工具查看 dll 和 exe 的依赖文件

假设安装的是 VS2015，打开 `VS201x x86 Native Tools Command Prompt` 

在该环境下使用如下命令即可查看

```
dumpbin /dependents 文件路径
```

（文件可直接拖入 cmd，即可得到文件路径）