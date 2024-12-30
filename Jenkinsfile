pipeline {
    agent any

    tools {
        nodejs 'nodejs'  // Ensure NodeJS is installed
        git 'Default'    // Ensure Git is configured
    }

    environment {
        PATH = "${tool 'nodejs'}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout SCM') {
            steps {
                // Checkout code from Git repository
                git url: 'https://github.com/Nandy2907/assessment_2.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Run npm install to install dependencies
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
                    // Fix linting issues automatically
                    bat 'npm run lint --fix'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Add build and test steps here
                }
            }
        }
    }

    post {
        always {
            // Perform actions that should run after pipeline completion
            echo 'Pipeline finished.'
        }
    }
}
