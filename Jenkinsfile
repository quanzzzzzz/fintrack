pipeline {
    agent any

    tools {
        maven 'Maven_3.9.11'  // Tên Maven đã cấu hình
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
                bat 'docker-compose down --remove-orphans'
                bat 'docker rm -f backend frontend || echo "No existing containers to remove"'
                bat 'docker-compose up -d'
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Cleaning up...'
            bat 'docker-compose down --remove-orphans'
        }
    }
}
