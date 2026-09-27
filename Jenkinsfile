pipeline {
    agent any
    environment {
        REGISTRY = "docker.io/${DOCKER_USERNAME}"
        IMAGE_NAME = "server-lms-net"
        SERVER_HOST = "103.20.96.174"
        SERVER_USER = "root"
    }
    stages {
        stage('Checkout') {
            steps {
                echo "Checkout code thành công..."
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t docker.io/truongdev1510/$IMAGE_NAME:latest ."
            }
        }
        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    sh "docker push docker.io/$DOCKER_USER/$IMAGE_NAME:latest"
                }
            }
        }
    }
}