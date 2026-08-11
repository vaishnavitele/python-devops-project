pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting source code from GitHub...'
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'python -m pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'python -m pytest'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t python-devops-app:latest .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker stop python-devops-container || exit /b 0'
                bat 'docker rm python-devops-container || exit /b 0'
                bat 'docker run -d --name python-devops-container -p 5000:5000 python-devops-app:latest'
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}