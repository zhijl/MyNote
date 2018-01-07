# 远程访问Jupter notebook
## 步骤
1. 登录远程服务器
2. 生成配置文件
```
$jupyter notebook --generate-config
```
3. 生成密码
打开ipython，创建密文：
```
In [1]: from notebook.auth import passwd
In [2]: passwd()
Enter password: 
Verify password: 
Out[2]: 'sha1:ce23d945972f:34769685a7ccd3d08c84a18c63968a41f1140274'
```
复制密文`'sha1:ce23d945972f:34769685a7ccd3d08c84a18c63968a41f1140274'`

4. 修改默认配置文件
```
$vim ~/.jupyter/jupyter_notebook_config.py 
```
修改如下：
```
c.NotebookApp.ip='*'
c.NotebookApp.password = u'sha:ce...刚才复制的那个密文'
c.NotebookApp.open_browser = False
c.NotebookApp.port =8888 #随便指定一个端口
```
5. 启动Jupyter notebook
```
$jupyter notebook
```
6. 远程访问
此时可直接从本地浏览器直接访问`http://address_of_remote:8888`就可以看到jupyter的登陆界面
**address_of_remote**  为远程服务器IP地址 
-------
如果登陆失败，则有可能是服务器防火墙设置的问题，此时最简单的方法是在本地建立一个ssh通道：
* 建立ssh通道
在本地终端中输入
```
ssh username@address_of_remote -L127.0.0.1:1234:127.0.0.1:8888 
```
便可以在localhost:1234直接访问远程的jupyter