pipeline {
    agent any
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
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-registry:5000/spring-petclinic:${env.BUILD_ID}")
                }
            }
        }
        stage('Push to Registry') {
            steps {
                script {
                    def imageName = "192.168.56.102:5000/spring-petclinic:${env.BUILD_NUMBER}"
                    docker.withRegistry('http://192.168.56.102:5000') {
                        def customImage = docker.build(imageName)
                        customImage.push()
                        docker.image(imageName).tag('latest')
                        docker.image("${imageName.split(':')[0]}:latest").push()
                    }
                }
            }
        }
    }
}
