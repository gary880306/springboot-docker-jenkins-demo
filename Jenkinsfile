pipeline {
    agent any

    tools {
        maven 'maven'
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'master', description: '請輸入欲建置的分支名稱')
    }

    environment {
        IMAGE_NAME = "demo-app-${params.BRANCH_NAME}"
        CONTAINER_NAME = "demo-container-${params.BRANCH_NAME}"
        PORT = "8080"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: "*/${params.BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/gary880306/springboot-docker-jenkins-demo.git']]
                ])
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop $CONTAINER_NAME || true
                    docker rm $CONTAINER_NAME || true
                    docker run -d -p 8080:8080 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }
}