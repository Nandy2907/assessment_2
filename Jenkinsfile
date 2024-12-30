pipeline {
    agent any
    tools {
        // Automatically fetch Node.js tool from Jenkins global tools configuration
        nodejs 'nodejs'  // Ensure this matches the Node.js installation name in Jenkins
    }

    environment {
        // Ensure paths are correct, no need to hardcode them if using Jenkins tools
        SONAR_SCANNER_PATH = tool name: 'sonar-scanner', type: 'Tool'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Ensure the correct Node.js is available and install dependencies
                    echo 'Installing dependencies...'
                    bat 'npm install'
                    bat 'npm audit fix'
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    // Run ESLint with debug for better error tracking
                    echo 'Running linting...'
                    bat 'npm run lint -- --debug'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run build process (if applicable)
                    echo 'Running build...'
                    bat 'npm run build'
                }
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token')  // Use Jenkins credentials for security
            }
            steps {
                script {
                    echo 'Running SonarQube analysis...'
                    bat """
                    set PATH=%SONAR_SCANNER_PATH%;%PATH%
                    sonar-scanner ^
                        -Dsonar.projectKey=backend ^
                        -Dsonar.sources=. ^
                        -Dsonar.host.url=http://localhost:9000 ^
                        -Dsonar.token=%SONAR_TOKEN%
                    """
                }
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
