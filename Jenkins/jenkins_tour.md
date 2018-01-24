# Jenkins 使用说明

## 构建一个 Pipeline

Jenkins Pipeline 是由一系列用来集成和实现可持续部署流程的插件构成. 一个可持续部署流程是一个将项目程序从版本控制平台获取后, 测试并确保无误后发送用户或在线平台的过程.

Jenkins Pipeline 提供了一个代码形式的 `pipeline`, 通过提供的一系列可扩展的工具来实现从简单到复杂的部署流程. Jenkins Pipeline 的定义只需通过一个设定名称的文本文件来实现(该文件名为 Jenkinsfile), 该文件放置于版本控制项目仓库中.

可以按照下列步骤快速地建立一个pipeline:

1. 点击 Web UI 上的 **New Item** 按钮;
1. 填写一个 New Item 项目名(例如 My Pipeline), 然后选择 **Multibranch Pipeline**, 点 OK;
1. 点击 **Add Source** 按钮, 点击 Add Source 按钮, 选择你用的版本控制平台, 并填入具体内容;
1. 点击 **Save** 按钮进行保存.

下面是一些简单的 Jenkinsfile 的内容示例, 你可以进行简单的修改, 比如在 `sh` 语句处加入一些类似的内容.

在你配置完你的 Pipeline 之后, Jenkins 将会自动检测任何一个新分支或者你在项目仓库中创建的 Pull Request, 检测随后便开始运行特定的 `pipeline`.

### Jenkinsfile 例子预览

下面的例子针对了不同编程语言

#### Java

``` jenkinsfile
pipeline {
    agent { docker 'maven:3.3.3' }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}
```

#### Node.js / JavaScript

``` jenkinsfile
pipeline {
    agent { docker 'node:6.3' }
    stages {
        stage('build') {
            steps {
                sh 'npm --version'
            }
        }
    }
}
```

#### Ruby

``` jenkinsfile
pipeline {
    agent { docker 'ruby' }
    stages {
        stage('build') {
            steps {
                sh 'ruby --version'
            }
        }
    }
}
```

#### Python

``` jenkinsfile
pipeline {
    agent { docker 'python:3.5.1' }
    stages {
        stage('build') {
            steps {
                sh 'python --version'
            }
        }
    }
}
```

#### PHP

``` jenkinsfile
pipeline {
    agent { docker 'php' }
    stages {
        stage('build') {
            steps {
                sh 'php --version'
            }
        }
    }
}
```

## `pipeline` 中的多元环节 `steps`

`pipeline` 中设有一种多元环节的规则, `pipeline` 中的多元环节 `steps` 实现了项目中的编译、测试和应用部署流程. Jenkins Pipeline 以一种简单的方式组织多元环节, 辅助自动化流程的模型建模.

另外将 `step` 作为 `steps` 中的一个单一的命令以执行一个单一的动作. 当一个 `step` 执行成功后, 会自动执行下一个 `step`. 而当一个 `step` 没有执行正确, 意味着该 `pipeline` 失败, 并停止执行.

当所有 `pipeline` 的 `steps` 执行都成功后, 则意味着该 `pipeline` 被成功执行.

实例如下

### 针对 Linux/BSD 和 Mac OS 的 `steps` 示例

在类 Unix 平台, 使用 `sh` step 命令实现在 `pipeline` 中定义一个shell语句.

``` Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'echo "Hello World"'
                sh '''
                    echo "Multiline shell steps works too"
                    ls -lah
                '''
            }
        }
    }
}
```

### Windows 中的 `steps` 示例

在 Windows 平台, 应该使用 `bat` step 命令来实现在 `pipeline` 中定义一个批处理语句.

``` Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                bat 'set'
            }
        }
    }
}
```

### `steps` 中的 timeout/retry 示例

这里也有一些已经封装过的 `steps`命令, 比如 `retry` 命令, 适用于一些需要对程序再次执行直到成功执行的情景, 同时通过 `timeout` 命令来约束某些语句的执行的时间.

在下面的 `Deploy` stage 中, 尝试执行 flakey-deploy.sh 脚本3次, 每次的时间间隔为3分钟, 直到某一次执行成功. 如果超过3分钟, health-check.sh 脚本仍未执行完, 则整个 `pipeline` 在 `Deploy` 阶段将被判定为执行失败.

``` Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                retry(3) {
                    sh './flakey-deploy.sh'
                }

                timeout(time: 3, unit: 'MINUTES') {
                    sh './health-check.sh'
                }
            }
        }
    }
}
```

封装的 `steps` 命令如 `timeout` 和 `retry` 同样还可以嵌套包含它们自己, 如 `retry` 中包含 `timeout` 命令.

比如, 如果我们想反复尝试五次部署, 但是总共不能超过三分钟, 示例如下

``` Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Deploy') {
            steps {
                timeout(time: 3, unit: 'MINUTES') {
                    retry(5) {
                        sh './flakey-deploy.sh'
                    }
                }
            }
        }
    }
}
```

### 所有的 stages 执行完成之后

当 `pipeline` 中的所有阶段执行成功后, 你可能需要进行清理环节或基于 `pipeline` 的输出结果执行一些其他的动作, 那么这些动作可以在 `post` 节点中进行定义. 示例如

``` Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh 'echo "Fail!"; exit 1'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
```

## `pipeline` 中的执行环境

`pipeline` 中的 `agent` 将告诉 Jenkins 在怎样的环境执行该 `pipeline` 或者 `pipeline` 中的一部分过程. 而 `agent` 在每个 `pipeline` 中都是必须声明的.

这里有几种 `agent` 方式, 而这里的例子将主要以 Docker 环境为主进行讲述.

`pipeline` 可以很方便地结合 Docker 并且在 Docker 中执行. 而这需要 `pipeline` 事先定义好需要在什么环境中、需要什么工具, 而不是后续手动设置这些. 这种方式也使得在 Docker 容器中进行打包更加容易.

更多的细节, 可以参考后面的语法说明章节.

简单的例子如下:

``` Jenkinsfile
pipeline {
    agent {
        docker { image 'node:7-alpine' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
            }
        }
    }
}
```

当 `pipeline` 执行的时候, 将自动启动特殊的容器并执行预先定义的 `steps`.

Jenkins 输出端将输出如下信息:

``` text
[Pipeline] stage
[Pipeline] { (Test)
[Pipeline] sh
[guided-tour] Running shell script
+ node --version
v7.4.0
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
```

`pipline` 中可以很容易地融合多个容器或者不同的 `agent`, 而关于其中更多的设置, 可以参考下面一节, 使用环境变量.

## `pipeline` 中的环境变量

环境变量可以设为全局的也可以时局部的针对某一个 `stage` 的.

在 `Jenkinsfile` 内定义环境变量, 这样就允许你在在 Jenkins 中对类似 `Makefile` 这样的构建文件做一些配置.

例如:

``` Jenkinsfile
pipeline {
    agent any

    environment {
        DISABLE_AUTH = 'true'
        DB_ENGINE    = 'sqlite'
    }

    stages {
        stage('Build') {
            steps {
                sh 'printenv'
            }
        }
    }
}
```

另外, 可以通过使用环境变量避免在脚本的多个地方重复插入相同的冗长的信息, 同时 `Jenkinsfile` 推荐使用环境变量来引用验证信息(如密钥), 确保了私密信息不直接显现在文本中.

例如:

``` Jenkinsfile
environment {
    AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
    AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
}
```

如果已经配置过验证信息, 如密钥或API tokens, 那么可以通过环境变量很轻易地调用它们.

另外一种方式, 可以在环境变量同时设定用户名和密码, 例如

``` Jenkinsfile
environment {
   SAUCE_ACCESS = credentials('sauce-lab-dev')
}
```

与用户名和密码相关的有三种环境变量:

- `SAUCE_ACCESS` 包含 `<username>:<password>`
- `SAUCE_ACCESS_USR` 包含 `username`
- `SAUCE_ACCESS_PSW` 包含 `password`

在下面一节, 我们介绍另一个重要的持续部署概念: 信息反馈

## `pipeline` 中的过程信息记录

这里指的过程信息, 包括编译、测试等阶段的中间日志输出, 其中较为关注的时测试结果信息的收集. 

测试是可持续部署过程中重要的一部分, 没有人想在成篇的输出信息中寻找某些失败的测试输出, 为了使这个过程容易, 当在运行测试结果时, Jenkins 可以记录并且过滤处测试结果, 保存在文件中. 例如, 通过插件我们可以将 Jenkins 和 `junit` 进行绑定, 使得可以以报告的形式对测试输出内容进行转换和整理.

在 `pipeline` 中我们通过使用 `post` 定义相应的动作.

例如

``` Jenkins
pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                sh './gradlew check'
            }
        }
    }
    post {
        always {
            junit 'build/reports/**/*.xml'
        }
    }
}
```

在上面这个例子中, Jenkins 将会抓取测试结果追溯有用的信息, 整理并形成报告. 如果一个 `pipeline` 中的测试项有部分是失败的, 那该 `pipeline` 将被标记为 `UNSTABLE`, 在 Web UI 界面中将以黄色标注显现. 这区别于 `pipeline` 失败时, 用红色进行标注.

## `pipeline` 中的清理和消息提醒

这一节继续上面的 `post` 节点, `post` 作为 `pipeline` 最后一个阶段被执行, 我们可以在其中加入一些消息提示或者完成一些清理工作.

例如

``` Jenkinsfile
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
            echo 'I failed :('
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
```

在 `post` 中有很多中选择, 如 `always` 、`success`、`unstable`、`failure` 和 `changed`, 对应了 `pipeline` 的不同执行状态.

关于消息提示, 例如发送邮件, 可以参考下面示例

``` Jenkinsfile
post {
    failure {
        mail to: 'team@example.com',
             subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
             body: "Something is wrong with ${env.BUILD_URL}"
    }
}
```

上面的示例中, 在 `pipeline` 构建失败的时候, 将按特定格式给制定电子邮箱发送电子邮件.

## `pipeline` 中的持续部署

一般持续部署框架都会有这三个阶段: 编译, 测试, 部署. 在这一节中我们将介绍的是部署阶段. 这一阶段需要基于稳定或成功的编译和测试.

一个基本的 `pipeline` 的框架:

``` Jenkinspipe
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
    }
}
```

### 设置部署环境

部署环境可能有多个, 比如处于开发阶段的 `staging`, 产品线上的 `production` 环境. 例如下面的代码片段:

``` Jenkinsfile
stage('Deploy - Staging') {
    steps {
        sh './deploy staging'
        sh './run-smoke-tests'
    }
}
stage('Deploy - Production') {
    steps {
        sh './deploy production'
    }
}
```

在这个例子体现的是, 无论我们后面要通过 run-smoke-tests 脚本进行怎样的冒烟测试, 在正式部署到产品线上时, 都要先通过一次质量检测, 才能正式上线. 整个 `pipeline` 都是全自动的, 但是有时我们想人为地在发行前做一次确认, 这是就需要程序自动发起请求, 寻求确认.

### `pipeline` 处理过程中的人为确认

一般在部署阶段我们想要人为进行确认, 者只需在我们想要重点关注的阶段前加入一个 `input` step, 则 `pipeline` 运行到此将发起请求, 待人确认后再继续进行.

例如

``` Jenkinsfile
pipeline {
    agent any
    stages {
        /* "Build" and "Test" stages omitted */

        stage('Deploy - Staging') {
            steps {
                sh './deploy staging'
                sh './run-smoke-tests'
            }
        }

        stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }

        stage('Deploy - Production') {
            steps {
                sh './deploy production'
            }
        }
    }
}
```

总结, 上面的章节作为一个入门指南带你了解 Jenkins 和 Jenkins Pipeline 的基础使用方法, 应为 Jenkins 时极易扩展的, 可以被轻易修改和配置实现各种自动化. 想要获知更多内容, 可以参考 [User Handbook](https://jenkins.io/doc/book).

## 参考

[1] [Jenkins document](https://jenkins.io/doc)

> Translate by zhijinlin
> Date: 2018年01月24日11:35:23
