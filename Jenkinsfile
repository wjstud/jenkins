pipeline {

    # 指定一下的操作都在标签为 dev 这台机器中执行
    agent {
        node {
            label 'dev'
        }
    }

    # 设置一些环境变量来控制并不是每次都需要执行的步骤
    environment {
        CREATE_DB = 'no'
        UPDATE_DB = 'no'
        UPDATE_ENV = 'no'
        UPDATE_JS = 'no'
        RUN_SCRIPT = 'no'
    }

    stages {

        # 更新开发环境中的代码
        stage('pull source code ') {
            steps {
                # credentialsId 指定的是已经创建好的登录 github 账号的账号管理证书
                git url: 'git@github.com:yingque/test_git.git', branch: 'master', credentialsId: 'd4eb2031-854e-xxxx-a7d8-da5b56989a63'
            }
        }

        # 更新开发环境中 python 的依赖包
        stage('update envirement') {
            # 通过判断 UPDATE_ENV 环境变量的值来决定是否更新 python 的相关依赖裤，因为只有在更新的时候才需要
            when {
                environment name: 'UPDATE_ENV',
                value: 'yes'
            }
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        # 若是项目中有 js 需要重新编译，可以通过 UPDATE_NPM 值来决定是否需要更新依赖裤与编译 js
        stage('install app js lib') {
            when {
                environment name: 'UPDATE_NPM',
                value: 'yes'
            }
            steps {
                sh 'cd app && npm install --registry https://registry.npm.taobao.org && npm run build'
            }
        }

        # 若是初次建立我们的开发环境，我们可能需要建立我们的数据库
        stage('create database') {
            when {
                environment name: 'CREATE_DB',
                value: 'yes'
            }
            steps {
                sh 'python manage.py database create'
            }
        }

        # 有时候会有一些特殊的操作，需要执行我们已经写好的脚本，如数据库的 patch 等等
        stage ('run python script') {
            when {
                environment name: 'RUN_SCRIPT',
                value: 'yes'
            }
            steps {
                sh 'python script/patch.py'
            }
        }

    }

    # 在所有的 stage 操作执行之后会出发 post 指令，若是都执行成功则出发 success 指令，若是其中有一步错误了就执行 failure 指令
    post {

        # 成功执行之后通过邮件模块发送邮件通知相关人员
        success {
            emailext body:
                body: "This job is success",
                subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) is success :)",
                to: "support@shiyanlou.com"
        }

        # 执行失败也通过邮件模块发送邮件通知相关人员
        failure {
            emailext body:
                body: "This job is fail",
                subject: "Job '${env.JOB_NAME}' (${env.BUILD_NUMBER}) is fail :(",
                to: "support@shiyanlou.com"
        }
    }
}

来源: 实验楼
链接: https://www.shiyanlou.com/courses/1032
本课程内容，由作者授权实验楼发布，未经允许，禁止转载、下载及非法传播
