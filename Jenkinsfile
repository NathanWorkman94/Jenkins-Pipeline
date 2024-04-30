pipeline {
    agent any
    environment {
        DIRECTORY_PATH = './directory'
        TESTING_ENVIRONMENT = 'test-env'
        PRODUCTION_ENVIRONMENT = 'Nathan'
    }

    stages {
        stage('Build') {
            steps {
                echo 'fetch the source code from the directory path specified by the environment variable'
                echo 'compile the code and generate any necessary artifacts'
            }
        }
        stage('Test') {
            steps {
                echo 'unit tests'
                echo 'integration tests'
            }
        }
        stage('Code Quality Check') {
            steps {
                echo 'check the quality of the code'
            }
        }
        stage('Deploy') {
            steps {
                echo 'deploy the application to a testing environment specified by the environment variable'
            }
        }
        stage('Approval') {
            steps {
                sleep time:10, unit:'SECONDS'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Deploying to $PRODUCTION_ENVIRONMENT'
            }
        }
    }
}