pipeline {
    agent any

    tools {
        git 'Default'
    }

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
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    bat 'snyk auth %SNYK_TOKEN%'     
                    bat 'npm test'                   
                }
            }
        }

        stage('Run Coverage') {
            steps {
                bat 'npm run coverage'
            }
        }

        stage('Security Scan') {
            steps {
                withCredentials([string(credentialsId: 'SNYK_TOKEN', variable: 'SNYK_TOKEN')]) {
                    bat 'snyk test'                 
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
    }
}