## 在使用 Drone CI 配置 Windows 平台的 exec runner 时遇到的问题

Windows 下 PowerShell 默认的权限级别是 Restricted，不允许执行 PS 脚本（即 .ps1 文件）。如果在 Restricted 权限级别下运行，会得到错误信息：

``` text
.\XXXX.ps1 : File
XXXX.ps1 cannot be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at http://go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ .\XXXX.ps1 params[] ...
+ ~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

要解决这类问题，通常的做法是，用管理员权限启动 PS 命令行，将执行权限修改为 RemoteSigned 或者 Unrestricted：

``` text
Set-ExecutionPolicy RemoteSigned
```

ref: https://www.cnblogs.com/urwlcm/p/4333119.html
