pipeline {
    agent any

    tools {
        nodejs "NodeJSInstallation"  // Ensure NodeJS is installed on Jenkins
        git "Git" // Ensure Git is installed on Jenkins
    }

    environment {
        PATH = "${tool 'NodeJSInstallation'}/bin:${env.PATH}"
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
                    // Installing npm dependencies
                    bat 'npm install'
                }
            }
        }

        stage('Lint') {
            steps {
                script {
                    // Running linting checks
                    bat 'npm run lint'
                }
            }
        }

        stage('Fix Lint Issues') {
            steps {
                script {
                    // Automatically fix linting issues if possible
                    bat 'npm run lint --fix'
                }
            }
        }

        stage('Build and Test') {
            steps {
                script {
                    // Your build and test steps here, e.g., running tests, building artifacts, etc.
                }
            }
        }
    }

    post {
        always {
            // Steps to always run, like cleaning up or sending notifications
        }
    }
}
