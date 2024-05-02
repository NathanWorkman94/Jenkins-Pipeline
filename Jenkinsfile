pipeline {
    agent any
    environment {
        DIRECTORY_PATH = './directory'
        TESTING_ENVIRONMENT = 'test-env'
        PRODUCTION_ENVIRONMENT = 'Nathan'
        AWS_ACCESS_KEY_ID = 'my_access_key'
        AWS_SECRET_ACCESS_KEY = 'my_secret_key'
        AWS_DEFAULT_REGION = 'au-east-1'
        EC2_INSTANCE_ID = 'instance_id'
        STAGING_URL = 'http://mystagingurlexample.com'
        S3_BUCKET = 'my-product-bucket'
    }

    stages {
        stage('Build') {
            steps {
                echo 'Starting build process using Maven...'
                
                // Fetch the latest code from the specified directory (assuming code is checked out prior to this stage)
                echo 'Fetching source code from $DIRECTORY_PATH'
                
                // Example of in real scenario how I would clean previous build outputs, compile the source code and package it
                echo 'sh \'mvn clean package -DskipTests\''
                
                echo 'Build completed successfully, artifact is packaged'
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Starting Unit and Integration Tests using PyTest...'
                // Example demonstrating the PyTest command that would be used
                echo 'pytest'
                echo 'Unit and integration tests completed'
            }
            post {
                success {
                    echo 'Attempting to send a basic notification email for Security Scan...'
                    mail to: "nathanworkman94@gmail.com",
                    subject: "Simple Test: ${env.JOB_NAME} - ${env.BUILD_NUMBER}",
                    body: "Simple test email to check connectivity issues.",
                    )
                }
            }
        stage('Code Analysis') {
            steps {
                echo 'Starting Code Analysis using SonarQube...'
                
                // Example demonstration of the SonarQube command that would be used
                echo 'sonar-scanner'
                
                echo 'Code analysis completed, check SonarQube dashboard for details'
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Starting Security Scan using OWASP ZAP...'
                
                // Example demonstration of the OWASP ZAP scan command that would be used
                echo 'zap-cli quick-scan --self-contained --target http://mycoolwebsitelinkwouldgohere.com'
                
                echo 'Security scan completed, check OWASP ZAP results'
            }
            post {
            always {
                echo 'Sending notification email for Security Scan...'
                emailext (
                    to: 'nathanworkman94@gmail.com',
                    subject: "Jenkins Notification: ${env.JOB_NAME} - ${env.BUILD_NUMBER} - Security Scan Result",
                    body: """<p>Security Scan completed. Please find the details below:</p>
                            <p><strong>Status:</strong> ${currentBuild.currentResult}</p>
                            <p>See attached logs for more details.</p>""",
                    mimeType: 'text/html'
                )
            }
        }
    }
        stage('Deploy to Staging') {
            steps {
                echo 'Deploying to AWS EC2 staging server...'
                echo 'Uploading application artifact...'
                echo 'Example AWS CLI command: aws s3 cp $DIRECTORY_PATH/my-app.zip s3://my-bucket-name/'
                
                // Deploying
                echo 'Updating EC2 instance with new application code...'
                echo 'Example AWS CLI command: aws ssm send-command --instance-ids $EC2_INSTANCE_ID --document-name "AWS-RunShellScript" --parameters commands="sudo docker pull myrepo/myapp:latest && sudo docker run -d myrepo/myapp:latest"'
                
                echo 'Deployment to staging completed successfully'
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Starting Integration Tests on the Staging environment...'
                
                // Example to show using Newman to run Postman collections against the staging server
                echo 'Running integration tests using Newman with Postman collections...'
                echo 'Example command to run Newman: newman run Postman_collection.json -e Staging_Environment.postman_environment.json --reporters cli,html --reporter-html-export test-report.html'
                
                echo 'Checking test results and logs...'
                echo 'cat test-report.html'
                
                echo 'Integration tests on staging completed successfully, check test reports for details'
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Starting deployment to production server...'
                
                // Upload application artifact to an S3 bucket
                echo "Example AWS CLI command: aws s3 cp ${env.DIRECTORY_PATH}/my-app.zip s3://${env.S3_BUCKET}/"
                
                // Update EC2 instance with new application code
                echo 'Updating EC2 with new application code...'
                echo "Example AWS CLI command: aws ssm send-command --instance-ids ${env.EC2_INSTANCE_ID} --document-name 'AWS-RunShellScript' --parameters commands='sudo docker pull myrepo/myapp:latest && sudo docker run -d myrepo/myapp:latest'"
                
                echo 'Deployment to production completed successfully'
            }
        }
    }
}
