pipeline {
    agent any

    tools {
        git 'Default'  // Use the Git installation you set in Jenkins
        nodejs 'NodeJS' // Make sure NodeJS is configured in Jenkins Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
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