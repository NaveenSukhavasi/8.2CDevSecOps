pipeline {
    agent any

    environment {
        SNYK_TOKEN = '0a4a442f-4e26-45ba-8898-2ab61e4720d7'
        EMAIL_RECIPIENT = 'naveensukhavasi@gmail.com'
        EMAIL_FROM = 'naveensukhavasi@gmail.com'  // must match SMTP credentials
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
                        from: "${EMAIL_FROM}",
                        to: "${EMAIL_RECIPIENT}",
                        subject: "SUCCESS: Snyk Scan - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The Snyk Scan stage completed successfully for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check console output: ${env.BUILD_URL}console""",
                        attachLog: true
                    )
                }
                failure {
                    emailext (
                        from: "${EMAIL_FROM}",
                        to: "${EMAIL_RECIPIENT}",
                        subject: "FAILURE: Snyk Scan - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The Snyk Scan stage failed for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check console output: ${env.BUILD_URL}console""",
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
                    sonar-scanner -Dsonar.token=%SONAR_TOKEN%
                    """
                }
            }
            post {
                success {
                    emailext (
                        from: "${EMAIL_FROM}",
                        to: "${EMAIL_RECIPIENT}",
                        subject: "SUCCESS: SonarCloud Analysis - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The SonarCloud Analysis stage completed successfully for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check console output: ${env.BUILD_URL}console""",
                        attachLog: true
                    )
                }
                failure {
                    emailext (
                        from: "${EMAIL_FROM}",
                        to: "${EMAIL_RECIPIENT}",
                        subject: "FAILURE: SonarCloud Analysis - Build ${env.BUILD_NUMBER}",
                        body: """Hello Naveen,

The SonarCloud Analysis stage failed for project ${env.JOB_NAME} (Build #${env.BUILD_NUMBER}).

Check console output: ${env.BUILD_URL}console""",
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