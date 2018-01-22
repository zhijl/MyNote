# Jenkins 邮箱配置

本文针对 Jenkins 2.89.3

## 配置方法

可参考[1], 具体为
在 Jenkins 的 Manage Jenkins -> Configure System 中进行设置, 修改项为

- **Jenkins Location** 中的 `System Admin e-mail address` 修改为你想设定的默认发送方邮箱账号: 如123@qq.com
- **E-mail Notification** 中的
  - `SMTP server` 设置为邮箱SMTP服务器域名, 如 `smtp.qq.com`
  - `Default user e-mail suffix` 设置为邮箱后缀, 如 `@qq.com`
  - 勾选 `Use SMTP Authentication` 在后面的 `User Name` 和 `Password` 填上发送方邮箱和密码(对于qq邮箱, 还需要登录邮箱, 在设置中将 **POP3|SMTP服务** 开启, 开启后可能提示用一个信息密钥作为SMTP服务的登录密码, 这时需要用该密钥作为登录密码)
  - 勾选 `Use SSL`
  - `SMTP Port` 中填写465 (smtp.qq.com 服务器端口)
  - `Reply-To Address` 填发送方邮箱账号
  - `Charset` 填 UTF-8

然后可以勾选后面的 `Test configuration by sending test e-mail` 进行邮件发送测试

## 在 Pipeline 中进行配置

可参考[2], 具体示例为

**Jenkinsfile**:

``` text
pipeline {
    agent any
    stages {
        stage('No-op') {
            steps {
                sh 'ls'
            }
        }
    }
    post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'I succeeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            mail to: 'team@example.com',
                subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                body: "Something is wrong with ${env.BUILD_URL}"
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
```

## 参考

[1] [jenkins配置邮箱服务器](http://blog.csdn.net/dingshuangyong/article/details/77530461)
[2] [Cleaning up and notifications](https://jenkins.io/doc/pipeline/tour/post/)