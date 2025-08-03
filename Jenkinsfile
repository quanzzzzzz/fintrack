pipeline {
    agent any

    tools {
        maven 'Maven 3.9.11'  // ⚠️ Phải đúng với tên Maven bạn đã thêm trong "Global Tool Configuration"
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
                bat 'docker-compose down'
                bat 'docker-compose up -d'
            }
        }
    }
}
