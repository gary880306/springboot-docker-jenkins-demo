pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'master', description: '請輸入要部署的 Git 分支名稱')
    }

    tools {
        maven 'maven'
    }

    environment {
        IMAGE_NAME = "demo-app"
        CONTAINER_NAME = "demo-container"
        PORT = "8080"
    }

    stages {
        stage('Clean Old Container') {
            steps {
                sh '''
                    docker ps -aq --filter name=$CONTAINER_NAME | xargs -r docker rm -f
                    docker images -q $IMAGE_NAME | xargs -r docker rmi -f
                '''
            }
        }

        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: "*/${params.BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/gary880306/springboot-docker-jenkins-demo']]
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
                    docker run -d -p $PORT:8080 --name $CONTAINER_NAME $IMAGE_NAME
                '''
            }
        }
    }
}