pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main')
        string(name: 'BUILD_ENV', defaultValue: 'dev')
        booleanParam(name: 'RUN_TESTS', defaultValue: true)
    }

    environment {
        NEW_VERSION = "1.0.2"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Checking out branch: ${params.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing required packages..."
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                echo "Building version ${env.NEW_VERSION} for ${params.BUILD_ENV} environment"
                bat '''
                    echo Simulating build process...
                    if not exist build mkdir build
                    copy *.js build
                    echo Build completed successfully!
                    echo App version: ${NEW_VERSION} > build\\version.txt
                '''
            }
        }

        stage('Test') {
            when {
                expression { return params.RUN_TESTS }
            }
            steps {
                echo "Running Jest tests..."
                bat 'npx jest'
            }
        }

        stage('Package') {
            when {
                expression { return currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo "Packaging build artifacts..."
                bat 'echo Packaging completed successfully.'
            }
        }

        stage('Deploy (Simulation)') {
            when {
                expression { return currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                echo "Deploying build version ${NEW_VERSION} (simulation)..."
                bat 'echo Deployment simulated successfully.'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            deleteDir()
        }
        failure {
            echo 'Pipeline failed! Check console output for details.'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}
