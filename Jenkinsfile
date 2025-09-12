pipeline {
    agent any

    environment {
        SNYK_TOKEN = '0a4a442f-4e26-45ba-8898-2ab61e4720d7'
        EMAIL_RECIPIENT = 'naveensukhavasi@gmail.com'
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
            post {
                success {
                    emailext (
                        subject: "SUCCESS: Snyk Scan - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The Snyk Scan stage completed successfully for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check the console output at: ${env.BUILD_URL}console""",
                        to: "${EMAIL_RECIPIENT}",
                        attachLog: true
                    )
                }
                failure {
                    emailext (
                        subject: "FAILURE: Snyk Scan - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The Snyk Scan stage failed for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check the console output at: ${env.BUILD_URL}console""",
                        to: "${EMAIL_RECIPIENT}",
                        attachLog: true
                    )
                }
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat """
                    REM Run SonarScanner via npm
                    sonar-scanner -Dsonar.login=%SONAR_TOKEN%
                    """
                }
            }
            post {
                success {
                    emailext (
                        subject: "SUCCESS: SonarCloud Analysis - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The SonarCloud Analysis stage completed successfully for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check the console output at: ${env.BUILD_URL}console""",
                        to: "${EMAIL_RECIPIENT}",
                        attachLog: true
                    )
                }
                failure {
                    emailext (
                        subject: "FAILURE: SonarCloud Analysis - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The SonarCloud Analysis stage failed for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check the console output at: ${env.BUILD_URL}console""",
                        to: "${EMAIL_RECIPIENT}",
                        attachLog: true
                    )
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