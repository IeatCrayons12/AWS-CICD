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
        stage('Deploy to ECS') {
            steps {
                script {
                    echo "Updating ECS service: $ECS_SERVICE in cluster: $ECS_CLUSTER"

                    sh """
                    # Login to AWS ECR
                    aws ecr get-login-password --region $AWS_REGION | \
                        docker login --username AWS --password-stdin $ECR_REPO

                    # Pull the latest image from ECR
                    docker pull $ECR_REPO:latest

                    # Update ECS service to force new deployment
                    aws ecs update-service \
                        --cluster $ECS_CLUSTER \
                        --service $ECS_SERVICE \
                        --force-new-deployment \
                        --region $AWS_REGION
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment completed successfully!"
        }
        failure {
            echo "Deployment failed. Check logs for details."
        }
    }
}
