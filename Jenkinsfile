pipeline {
    agent any
    tools {
        maven 'mvn'  // Use the name exactly as configured in Jenkins
    }
    
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
                    withDockerRegistry([ credentialsId: 'dockerhub', url: '' ])  {
                        def image = docker.build("${IMAGE_NAME}")
                        image.push()
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
                sh "docker run -d -p 8000:8080 ${IMAGE_NAME}"
            }
        }
    }
}
