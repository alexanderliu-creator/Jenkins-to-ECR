pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                    checkout scm
                    // 打印分支名
                    echo "Branch name: ${env.BRANCH_NAME}"
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                    app = docker.build("tamar-jenkins-test")
                    echo "Docker image built: tamar-jenkins-test"
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
        stage('Deploy') {
            steps {
                script{
                    // 检查分支名是否为空
                    if (env.BRANCH_NAME == null) {
                        echo "BRANCH_NAME environment variable is not set. Defaulting to 'unknown-branch'."
                        env.BRANCH_NAME = 'unknown-branch'
                    }

                    def commitID = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
                    def buildTime = new Date().format("yyyyMMddHHmm", TimeZone.getTimeZone('UTC'))
                    def newTag = "${commitID}-${buildTime}"
                    
                    // 打印新的标签
                    echo "New tag to be pushed: ${newTag}"
                    echo "Hello World"

                    // 使用新的镜像标签进行推送
                    docker.withRegistry('https://186296540553.dkr.ecr.us-west-2.amazonaws.com/tamar-jenkins-test', 'ecr:us-west-2:181266c6-4c43-4088-bd78-cf889a1643e7') {
                        app.push(newTag)
                        app.push("latest")
                        echo "Images pushed: ${newTag} and latest"
                    }
                }
            }
        }
    }
}
