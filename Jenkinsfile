pipeline {
    agent any
    tools {
        nodejs 'nodejs' // Define your nodejs tool
    }

    environment {
        NODEJS_HOME = 'C:\\Program Files\\nodejs' // Set the Node.js installation path
        SONAR_SCANNER_PATH = 'C:\\Users\\senth\\Downloads\\sonar-scanner-cli-6.2.1.4610-windows-x64\\sonar-scanner-6.2.1.4610-windows-x64\\bin' // Set SonarQube scanner path
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
                npm install
                '''
            }
        }

        stage('Lint') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                echo "Current directory: %cd%"
                npm run lint -- --debug
                '''
            }
        }

        stage('Build') {
            steps {
                bat '''
                set PATH=%NODEJS_HOME%;%PATH%
                echo "Current directory: %cd%"
                npm run build
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                bat '''
                set PATH=%SONAR_SCANNER_PATH%;%PATH%
                where sonar-scanner || echo "SonarQube scanner not found. Please install it."
                sonar-scanner ^
                                  -Dsonar.projectKey=backend ^  // Use the project key provided
                                  -Dsonar.sources=. ^  // Analyze all files in the root of the repository
                                  -Dsonar.host.url=http://localhost:9000 ^  // Replace with your SonarQube instance URL
                                  -Dsonar.token=sqp_5dbf2635e6715825a74aea59d0b6729dca5000c7  // Use the token you provided
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
