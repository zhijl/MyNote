## Ubuntu 字体安装更改系统字体
Ubuntu默认字体为Ubuntu Regular

### 查看系统字体
Unity Tweak Tool查看字体

![Unity Tweak Tool查看字体](/_Resource/ubuntu_font_install_01.png)

### 安装字体
将字体拷贝到`/usr/share/fonts`目录
```
sudo cp * /usr/share/fonts
```
执行下列命令
```
sudo mkfontscale
sudo mkfontdir
sudo fc-cache -fv
```
### 修改系统字体
使用Unity Tweak Tool这个工具就能实现Ubuntu平台的系统字体修改。




2017年11月23日21:59:34

* filename: ubuntu_font_install

