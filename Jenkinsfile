pipeline {
    agent {
        docker {
            image 'docker:dind'
             args '-u root:root -p 3000:3000 --privileged -v /var/run/docker.sock:/var/run/docker.sock'
             }
           }

    environment {
        ECR_REPO_URL = "public.ecr.aws/w8c6h7o9/assessment"
        AWS_REGION = "us-east-1"
        ECR_CREDENTIALS = "aws_creds"
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
