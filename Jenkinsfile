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
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }

        stage('Run Coverage') {
            steps {
                sh 'npm run coverage'
            }
        }

        stage('Security Scan') {
            steps {
                sh 'snyk test'
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}