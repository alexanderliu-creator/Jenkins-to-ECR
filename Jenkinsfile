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
                }
            }
        }

        stage('Build') { 
            steps { 
                script{
                    app = docker.build("tamar-jenkins-test")
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
                    // 获取分支名和当前时间
                    def branchName = env.BRANCH_NAME.replaceAll('/', '-')
                    def buildTime = new Date().format("yyyyMMddHHmm", TimeZone.getTimeZone('UTC'))
                    def newTag = "${branchName}-${buildTime}"
                    
                    // 使用新的镜像标签进行推送
                    docker.withRegistry('https://186296540553.dkr.ecr.us-west-2.amazonaws.com/tamar-jenkins-test', 'ecr:us-west-2:181266c6-4c43-4088-bd78-cf889a1643e7') {
                        app.push(newTag)
                        app.push("latest")
                    }
                }
            }
        }
    }
}
