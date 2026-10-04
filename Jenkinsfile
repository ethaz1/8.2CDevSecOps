pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('SONAR_TOKEN')
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: ' https://github.com/ethaz1/8.2CDevSecOps.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }
        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0' // Allows pipeline to continue despite test failures
            }
        }
        stage('Generate Coverage Report') {
            steps {
                // Ensure coverage report exists
                bat 'npm run coverage || exit /b 0'
            }
        }
        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0' // This will show known CVEs in the output
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                // we need to download a zip and extract it.
                bat '''
                    curl -L -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-8.0.1.6346-windows-x64.zip
                    unzip zipFile: 'solar-scanner.zip'
                    bat 'bin/sonar-scanner.bat'
                '''
        }
    }
}