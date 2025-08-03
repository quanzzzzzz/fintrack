pipeline {
    agent any

    tools {
        maven 'Maven_3.9.11'  // ⚠️ Phải đúng với tên Maven bạn đã thêm trong "Global Tool Configuration"
    }

    environment {
        IMAGE_TAG = "latest"
    }

    stages {
        stage('Code Analysis - SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvn -f be-fintrack-master/pom.xml clean verify sonar:sonar -DskipTests'
                }
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker-compose build'
            }
        }

        stage('Deploy Local') {
            steps {
                // Dừng & dọn container đang chạy (tránh lỗi Conflict)
                bat 'docker-compose down --remove-orphans'
                // Xoá container backend nếu vẫn còn (an toàn)
                bat 'docker rm -f backend || echo "No existing backend container to remove"'
                // Khởi động lại
                bat 'docker-compose up -d'
            }
        }
    }
}
