pipeline {
    agent any

    environment {
        SNYK_TOKEN = '0a4a442f-4e26-45ba-8898-2ab61e4720d7'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/NaveenSukhavasi/8.2CDevSecOps'
            }
        }

        stage('Check Node Version') {
            steps {
                bat 'node -v'
            }
        }

        stage('Run Snyk Scan') {
            steps {
                bat """
                snyk auth %SNYK_TOKEN%
                snyk test
                """
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}