## 使用 Anaconda 中遇到的问题

问题：创建了一个新的 python3 环境，但是执行 python 时，调用的是 `anaconda/bin/python`，而该 python 是 2.7 版本的。执行 python3 可以正常调用该环境中的 python 3.x 版本。使用 pip 安装的库也是针对该 2.7 版本的。即无法正常给该环境中的 python3 安装第三方库

解决：`anaconda/envs/xxx/bin/pip` 使用该 pip 进行安装第三方库。
