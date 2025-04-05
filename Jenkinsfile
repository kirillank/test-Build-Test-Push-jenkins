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
                    docker.withRegistry('http://192.168.56.102:5000') {
                        docker.image("my-registry:5000/spring-petclinic:${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }
}
