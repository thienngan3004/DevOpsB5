pipeline {
    agent any

    environment {
        IMAGE_NAME = "thienngan/server-lms-net:latest"
        COMPOSE_FILE = "docker-compose.prod.yml"
    }

    stages {
        stage('1. Checkout Code') {
            steps {
                echo 'Đang lấy mã nguồn mới nhất từ Git...'
                checkout scm
            }
        }

        stage('2. Build Docker Image') {
            steps {
                echo 'Đang tiến hành build Docker image...'
                // Build image từ Dockerfile tại thư mục gốc
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }

        stage('3. Deploy with Docker Compose') {
            steps {
                echo 'Đang deploy container mới...'
                // Dừng container cũ và bật container mới chạy ngầm
                sh "docker compose -f ${COMPOSE_FILE} down"
                sh "docker compose -f ${COMPOSE_FILE} up -d"
            }
        }
    }

    post {
        success {
            echo '🎉 Deploy thành công rực rỡ! API đã sẵn sàng tại port 3007.'
        }
        failure {
            echo '❌ Deploy thất bại! Kiểm tra lại log của Jenkins.'
        }
    }
}