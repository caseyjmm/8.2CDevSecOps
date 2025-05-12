mpipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                bat 'git clone -b main https://github.com/caseyjmm/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Windows-friendly way to continue after test failures
            }
        }
        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                    echo Downloading SonarScanner CLI...
                    curl -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip

                    powershell -Command "Expand-Archive -Path sonar-scanner.zip -DestinationPath sonar"

                    echo Running SonarCloud Analysis...
                    .\\sonar\\sonar-scanner-5.0.1.3006-windows\\bin\\sonar-scanner.bat -Dsonar.login=%SONAR_TOKEN%
                    '''
                }
            }
        }
    }
}
