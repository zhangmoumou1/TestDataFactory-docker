pipeline {
    agent {
        label 'docker'
    }
    options {
        timestamps()
    }
    environment {
        GITHUB_USER_ID = '2b98d5a0-65f8-4961-958d-ad3620541256'
        ALIYUN_USER_ID = '06989ce7-86fb-43ca-aec0-313d260af382'
        IMAGE_NAME = 'ccr.ccs.tencentyun.com/zhangyancheng/zhangyancheng:1.0'
    }
    stages {
        stage('Clone sources') {
            options {
                timeout(time: 30, unit: 'SECONDS')
            }
            steps {
                git (credentialsId: "${GITHUB_USER_ID}", url: 'https://github.com/zhangmoumou1/TestDataFactory.git', branch: 'master')
            }
        }
        stage('Build image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}")
                }
            }
        }
        stage('Push image') {
            steps {
                withDockerRegistry(credentialsId: "${ALIYUN_USER_ID}", url: 'ccr.ccs.tencentyun.com/zhangyancheng/zhangyancheng') {
                    sh "docker push ${IMAGE_NAME}"
                }
            }
        }
    }
    post {
        always {
            sh "docker images|grep '<none>'|awk '{print \$3}'|xargs docker image rm > /dev/null 2>&1 || true"
            sh "docker images"
            cleanWs()
        }
    }
}