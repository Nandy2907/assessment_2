pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs'
        SONAR_SCANNER_PATH = 'C:\\Users\\senth\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm // Checks out the source code from the repository
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                echo "Current directory: %cd%"
                if exist backend (
                    cd backend && npm install
                ) else (
                    echo "backend directory not found"
                    exit 1
                )
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                echo "Current directory: %cd%"
                if exist backend (
                    cd backend && npm run lint -- --debug
                ) else (
                    echo "backend directory not found"
                    exit 1
                )
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                echo "Current directory: %cd%"
                if exist backend (
                    cd backend && npm run build
                ) else (
                    echo "backend directory not found"
                    exit 1
                )
                '''
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner ^
                              -Dsonar.projectKey=backend ^
                              -Dsonar.sources=backend ^
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
