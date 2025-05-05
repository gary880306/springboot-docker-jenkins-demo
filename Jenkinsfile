pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'master', description: 'Git 分支名稱')
    }

    tools {
        maven 'maven'
    }

    environment {
        IMAGE_NAME = "demo-app-${params.BRANCH_NAME}"
        CONTAINER_NAME = "demo-container-${params.BRANCH_NAME}"
        PORT = "${params.BRANCH_NAME == 'master' ? 8080 : (params.BRANCH_NAME == 'develop' ? 8081 : 8089)}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH_NAME}", url: 'https://github.com/gary880306/springboot-docker-jenkins-demo'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('Clean Old Containers and Images') {
            steps {
                // 停掉同名容器
                sh "docker ps -a -q --filter name=demo-container- | xargs -r docker rm -f"
                // 刪掉同名 image
                sh "docker images -q 'demo-app-*' | xargs -r docker rmi -f || true"
            }
        }

        stage('Deploy') {
            steps {
                sh "docker run -d -p ${PORT}:8080 --name ${CONTAINER_NAME} ${IMAGE_NAME}"
            }
        }
    }
}