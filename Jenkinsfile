pipeline {
    agent any

    tools {
        nodejs 'NodeJS'       
        git 'DefaultGit'      
    }

    parameters {
        booleanParam(name: 'RUN_AUDIT_FIX', defaultValue: false, description: 'Run npm audit fix?')
        booleanParam(name: 'RUN_SNYK_MONITOR', defaultValue: false, description: 'Push results to Snyk Dashboard?')
    }

    environment {
        SNYK_TOKEN = credentials('snyk-token') 
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/NaveenSukhavasi/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Unit Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Snyk Test') {
            steps {
                withCredentials([string(credentialsId: 'snyk-token', variable: 'SNYK_TOKEN')]) {
                    bat 'snyk auth %SNYK_TOKEN%'
                    bat 'snyk test'
                }
            }
        }

        stage('Snyk Monitor (Optional)') {
            when {
                expression { return params.RUN_SNYK_MONITOR == true }
            }
            steps {
                withCredentials([string(credentialsId: 'snyk-token', variable: 'SNYK_TOKEN')]) {
                    bat 'snyk auth %SNYK_TOKEN%'
                    bat 'snyk monitor'
                }
            }
        }

        stage('Audit Fix (Optional)') {
            when {
                expression { return params.RUN_AUDIT_FIX == true }
            }
            steps {
                bat 'npm audit fix --force'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/npm-debug.log', allowEmptyArchive: true
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}