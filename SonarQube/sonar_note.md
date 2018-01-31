# SonarQube 笔记

SonarQube 是一个开源的代码质量检测工具

## 安装

准备工作:

1. Java JDK
1. MySQL 数据库
1. SonarQube 下载地址: https://www.sonarqube.org/downloads/ SonarQube 6.7.1 Dec. 21. 2017

下载好 SonarQube 之后, 解压并进入 bin 目录, 启动相应 OS 目录下的 StartSonar. 例如在Ubuntu 64位系统平台, 启动方式如下:

``` shell
sonarqube/sonarqube-6.7.1/bin/linux-x86-64$ ./sonar.sh start
Starting SonarQube...
Started SonarQube.
```

启动浏览器，访问 http://localhost:9000 , 如果出现SonarQube界面, 说明安装成功.

最终界面如下:

![SonarQube](/_Resource/sonar_01.png)

## 配置

1. 打开 MySQL 数据库, 新建数据库
1. 打开 SonarQube 安装目录下的 conf\sonar.properties 文件, 在 mysql5.X 节点下输入以下信息
   ``` text
    sonar.jdbc.url=jdbc:mysql://192.168.1.119:3306/sonar?useUnicode=true&characterEncoding=utf8&rewriteBatchedStatements=true&useConfigs=maxPerformance
    sonar.jdbc.username=zhi
    sonar.jdbc.password=abc
    sonar.sorceEncoding=UTF-8
    sonar.login=zhi
    sonar.password=abc
   ```

   url 是数据库连接地址，username 是数据库用户名，jdbc.password 是数据库密码，login 是sonarqube 的登录名，sonar.password 是sonarqube的密码

   > 注: jdbc连接mysql数据库的url为:
jdbc:mysql://主机名或IP抵制:端口号/数据库名?useUnicode=true&characterEncoding=UTF-8

1. 重启服务器, 再次访问 http://localhost:9000 需要初始化数据库, 时间可能会有点长
1. 数据库初始化成功后，登录即可


[SonarQube官网](https://www.sonarqube.org/)
[获取mysql数据库jdbc链接地址](https://zhidao.baidu.com/question/305517185565764164.html)
[SonarQube的安装、配置与使用](https://www.cnblogs.com/qiaoyeye/p/5249786.html)