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

        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat """
                    REM Download SonarScanner CLI if not already downloaded
                    if not exist sonar-scanner-cli-5.15.0.36214-windows.zip (
                        powershell -Command "Invoke-WebRequest -Uri https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.15.0.36214-windows.zip -OutFile sonar-scanner-cli.zip"
                        powershell -Command "Expand-Archive -Path sonar-scanner-cli.zip -DestinationPath ."
                    )

                    REM Run SonarScanner
                    .\\sonar-scanner-5.15.0.36214-windows\\bin\\sonar-scanner.bat
                    """
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