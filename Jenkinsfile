pipeline {
    agent any

    environment {
        AWS_REGION = "ap-southeast-1"
        AWS_ACCOUNT_ID = credentials('aws-account-id')
        AWS_ACCESS_KEY_ID = credentials('aws-access-key-id')
        AWS_SECRET_ACCESS_KEY = credentials('aws-secret-access-key')
        ECR_REPO = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/aws-cicd-demo"
        ECS_CLUSTER = "aws-cicd-demo-cluster"
        ECS_SERVICE = "aws-cicd-demo-service"
    }

    stages {
        stage('Login to ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

        stage('Build & Push Docker Image') {
            steps {
                sh '''
                    docker build -t $ECR_REPO:latest .
                    docker push $ECR_REPO:latest
                '''
            }
        }

        stage('Deploy to ECS') {
            steps {
                sh '''
                    aws ecs update-service --cluster $ECS_CLUSTER \
                                           --service $ECS_SERVICE \
                                           --force-new-deployment
                '''
            }
        }
    }
}
