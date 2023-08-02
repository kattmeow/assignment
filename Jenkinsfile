pipeline {
    agent any

    environment {
        ECR_REPO_URL = $ECR_REPO_URL
        AWS_REGION = $AWS_REGION
        ECR_CREDENTIALS = $aws_creds
        IMAGE_NAME = 'assessment'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Dockerize the Python app
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // Log in to ECR
                    withAWS(region: AWS_REGION, credentials: ECR_CREDENTIALS) {
                        sh "docker tag $IMAGE_NAME:latest $ECR_REPO_URL/$IMAGE_NAME:latest"
                        sh "docker push $ECR_REPO_URL/$IMAGE_NAME:latest"
                    }
                }
            }
        }
    }
}
