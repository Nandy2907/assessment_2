pipeline {
    agent any

    tools {
        nodejs 'nodejs'  // Correct Node.js installation name
        git 'Default'    // Correct Git installation name (usually "Default" for the default Git installation)
    }

    environment {
        PATH = "${tool 'nodejs'}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout the code from Git
                git url: 'https://github.com/Nandy2907/assessment_2.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install npm dependencies
                    bat 'npm install'
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    // Run linting checks
                    bat 'npm run lint'
                }
            }
        }

        stage('Fix Lint Issues') {
            steps {
                script {
                    // Automatically fix linting issues
                    bat 'npm run lint --fix'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Add your build and test steps here
                }
            }
        }
    }

    post {
        always {
            // You can add cleanup steps here or leave it empty
            echo 'Pipeline finished.'
        }
    }
}
