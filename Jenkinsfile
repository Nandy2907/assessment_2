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
                checkout scm // Retrieves source code from the configured SCM
            }
        }

        stage('Install Dependencies') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run lint || exit /b 1
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                npm run build || exit /b 1
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
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner ^
                              -Dsonar.projectKey=mern ^  
                              -Dsonar.sources=. ^  
                              -Dsonar.host.url=http://localhost:9000 ^  
                              -Dsonar.token=%SONAR_TOKEN% ^
                              -Dsonar.verbose=true
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
        always {
            echo 'This block runs regardless of success or failure.'
        }
    }
}
