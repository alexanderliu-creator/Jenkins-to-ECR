pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    environment {
        // 获取当前分支名称
        CURRENT_BRANCH = env.BRANCH_NAME
        // 获取当前时间戳，格式为yyyyMMdd-HHmmss
        CURRENT_TIME = sh(script: 'date +%Y%m%d-%H%M%S', returnStdout: true).trim()
    }
    stages {
        stage('Clone repository') {
            steps {
                script {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    // 使用当前分支名称和时间构建镜像名称
                    image_name = "${CURRENT_BRANCH}_${CURRENT_TIME}"
                    app = docker.build("tamar-jenkins-test:${image_name}")
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Empty'
            }
        }
        stage('Deploy') {
            steps {
                script {
                    docker.withRegistry('https://186296540553.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:181266c6-4c43-4088-bd78-cf889a1643e7') {
                        app.push("${env.BUILD_NUMBER}")
                        // 推送带有分支名称和时间的镜像
                        app.push("tamar-jenkins-test:${image_name}")
                    }
                }
            }
        }
    }
}
