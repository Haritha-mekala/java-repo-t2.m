pipeline {
     agent { node { label 'agent1' } }
    tools {
        maven 'mvn' 
        jdk 'jdk'
    }
    
    environment {
        IMAGE_NAME = "Haritha/repo-1:${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Haritha-mekala/java-repo-t2.m.git'
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
                sh "docker run -d -p 8001:8080 ${IMAGE_NAME}"
            }
        }
    }
}
