pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Docker Build') {
            steps {
                sh 'docker build -t demo-app .'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop demo-container || true'
                sh 'docker rm demo-container || true'
                sh 'docker run -d -p 8080:8080 --name demo-container demo-app'
            }
        }
    }
}
