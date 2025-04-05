pipeline {
    agent any
    
    environment {
        REGISTRY = "192.168.56.102:5000"
        IMAGE_NAME = "spring-petclinic"
    }
    
    stages {
        stage('Build & Test') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        
        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Собираем образ с версионным тегом
                    def versionedImage = docker.build("${REGISTRY}/${IMAGE_NAME}:${env.BUILD_NUMBER}")
                    
                    // Пушим версионный образ
                    docker.withRegistry("http://${REGISTRY}") {
                        versionedImage.push()
                        
                        // Тегируем и пушим latest
                        versionedImage.tag('latest')
                        docker.image("${REGISTRY}/${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }
}
