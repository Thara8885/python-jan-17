pipeline {
    agent any

    environment {
        AWS_REGION = "us-east-1"
        ECR_REPO = "python-app"
        AWS_ACCOUNT_ID = "321209673437"
        IMAGE_TAG = "v1"
    }

    stages {

        stage("Code from Git") {
            steps {
                checkout scm
            }
        }

        stage("Build Docker Image") {
            steps {
                sh 'docker build -t $ECR_REPO:$IMAGE_TAG .'
            }
        }

        stage("Login to ECR") {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION | \
                docker login --username AWS --password-stdin \
                $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
                '''
            }
        }

        stage("Tag and Push Image") {
            steps {
                sh '''
                docker tag $ECR_REPO:$IMAGE_TAG \
                $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG

                docker push \
                $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/$ECR_REPO:$IMAGE_TAG
                '''
            }
        }
    }
}
