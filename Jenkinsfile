pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    environment {
        // 定义环境变量，用于构建镜像名称
        IMAGE_NAME = "${env.BRANCH_NAME}_${env.BUILD_ID}"
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
                    def dockerImage = docker.build("tamar-jenkins-test:${IMAGE_NAME}")
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
                script{
                    docker.withRegistry('https://186296540553.dkr.ecr.us-west-2.amazonaws.com/tamar-jenkins-test', 'ecr:us-west-2:181266c6-4c43-4088-bd78-cf889a1643e7') {
                        dockerImage.push("${env.BUILD_NUMBER}")
                        dockerImage.push("latest")
                    }
                }
            }
        }
    }
}
