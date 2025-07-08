pipeline {
    agent any
    environment {
        IMAGE_NAME = "srinivasulu2004/repo-1:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/srinivasulu2004/java-repo-t2.m.git'
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
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
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
