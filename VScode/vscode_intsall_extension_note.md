## VS Code 离线安装插件

遇到这样一种情况，Ubuntu 之前用得好好的 VS Code 突然之间插件 extension 商店打不开了，显示 `We cannot connect to the Extensions Marketplace at this time, please try again later.` 各种网上查找不到结果，之后决定折中处理这个问题，即考虑离线安装所需插件

方法如下：

1. 从官网下载所需插件 [https://marketplace.visualstudio.com/vscode](https://marketplace.visualstudio.com/vscode)

需要通过下列链接替换某些字段后进行下载：

```
https://${publisher}.gallery.vsassets.io/_apis/public/gallery/publisher/${publisher}/extension/${extension name}/${version}/assetbyname/Microsoft.VisualStudio.Services.VSIXPackage
```

例如下载：

```
https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph
```

下载链接为：

```
mhutchie.git-graph 
  => ${publisher} <-> mhutchie
  => ${extension_name} <-> git-graph
  => ${version}   <-> 1.7.0
```

```
https://mhutchie.gallery.vsassets.io/_apis/public/gallery/publisher/mhutchie/extension/git-graph/1.7.0/assetbyname/Microsoft.VisualStudio.Services.VSIXPackage
```

下载得到：

```
Microsoft.VisualStudio.Services.VSIXPackage
```

需重命名为 `.vsix` 后缀

1. 终端执行 `code --install-extension <path-to-vsix>` 即可
