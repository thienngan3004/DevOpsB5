pipeline {
    agent any
    environment {
        // Sử dụng trực tiếp username thienngan để đồng bộ
        DOCKER_USER_NAME = "thienngan"
        IMAGE_NAME = "server-lms-net"
        SERVER_HOST = "103.20.96.174"
        SERVER_USER = "root"
    }
    options {
        // Tắt checkout tự động của Jenkins để tránh xung đột
        skipDefaultCheckout()
    }
    stages {
        stage('Clean and Checkout') {
            steps {
                // Xóa sạch workspace trước, sau đó mới kéo code mới về
                cleanWs()
                checkout scm
            }
        }
        stage('Docker Build') {
            steps {
                sh "docker build -t docker.io/${DOCKER_USER_NAME}/$IMAGE_NAME:latest ."
            }
        }
        stage('Push Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred',
                    usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    // Đăng nhập và push lên đúng tài khoản thienngan
                    sh "echo '$DOCKER_PASS' | docker login -u '$DOCKER_USER' --password-stdin"
                    sh "docker push docker.io/${DOCKER_USER_NAME}/$IMAGE_NAME:latest"
                }
            }
        }
    }
}