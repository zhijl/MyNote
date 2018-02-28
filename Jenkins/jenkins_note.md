# Jenkins 介绍和安装

## 简介

Jenkins是一个开源软件项目，是基于Java开发的一种持续集成工具，用于监控持续重复的工作，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

### Jenkins 功能

1. 持续的软件版本发布/测试项目
1. 监控外部调用执行的工作

### Jenkins 特点

- Jenkins 可扩展的,基于组件的,开发者可以开发合适的组件搭配Jenkins对他们开发的软件进行各种编译、测试和部署
- Jenkins 是SPI的一部分(Software of Public Interest)
- 开源免费
- 跨平台，支持所有的平台
- master/slave 支持分布式的 build
- web形式的可视化的管理页面
- 安装配置超级简单
- tips及时快速的帮助
- 已有的200多个插件
- 可以接管整个软件的生命周期
- blue ocean面板
- 监控一些定时执行的任务

## Jenkins 安装

### 下载和启动

1. 官网下载 war 包

   Jenkins官网 https://jenkins.io/

1. 启动 war 包

环境要求:

- 大于 512 RAM, 大于 10GB 空间
- Java 8
- Docker (可选)

运行语句

``` shell
  java -jar jenkins.war --httpPort=8080
```

然后从浏览器访问 http://localhost:8080

首次访问界面提示需要输入 /home/zhi/.jenkins/secrets/initialAdminPassword 文件中的密钥, 输入后按提示操作即可完成后续安装的所有流程.

最终界面如下:

![Jenkins](/_Resource/jenkins_01.png)

## 参考

[1] [持续集成平台Jenkins的书籍或是学习资料](https://www.zhihu.com/question/29163932/answer/138366614)

[2] [Meet Jenkins](https://wiki.jenkins.io/display/JENKINS/Meet+Jenkins)

[3] [基于DOCKER的CI工具—DRONE](https://imnerd.org/drone.html)