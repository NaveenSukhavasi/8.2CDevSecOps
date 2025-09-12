pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NaveenSukhavasi/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Run Coverage') {
            steps {
                bat 'npm run coverage'
            }
        }

        stage('Security Scan') {
            steps {
                bat 'snyk test'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}