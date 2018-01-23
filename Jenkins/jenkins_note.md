# Jenkins 零碎笔记

## Jenkins 特点

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

## Jenkins 使用

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

从浏览器访问 http://localhost:8080

web 端访问界面提示需要输入 /home/zhi/.jenkins/secrets/initialAdminPassword 文件中的密钥
3b35864cc68d485195e72873c5ea2a1b

最终界面如下:

![Jenkins](/_Resource/jenkins_01.png)

## 参考

[持续集成平台Jenkins的书籍或是学习资料](https://www.zhihu.com/question/29163932/answer/138366614)

[Meet Jenkins](https://wiki.jenkins.io/display/JENKINS/Meet+Jenkins)

[基于DOCKER的CI工具—DRONE](https://imnerd.org/drone.html)