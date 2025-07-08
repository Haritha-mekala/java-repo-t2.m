pipeline {
    agent any
    environment {
        IMAGE_NAME = "your-dockerhub-username/java-hello-app:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-username/java-hello-app.git'
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Docker Build & Push') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials-id') {
                        def image = docker.build("${IMAGE_NAME}")
                        image.push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "docker run -d -p 8080:8080 ${IMAGE_NAME}"
            }
        }
    }
}
