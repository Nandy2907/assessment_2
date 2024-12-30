pipeline {
    agent any
    tools {
        nodejs 'nodejs'  // Ensure this matches the Node.js installation name in Jenkins
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs'
        SONAR_SCANNER_PATH = 'C:\\Users\\senth\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                npm audit fix
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npx eslint . --debug
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')
            }
            steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                sonar-scanner ^ 
                    -Dsonar.projectKey=backend ^ 
                    -Dsonar.sources=. ^ 
                    -Dsonar.host.url=http://localhost:9000 ^ 
                    -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
