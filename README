Developer commits code → GitHub
        ↓
GitHub Actions builds Docker image
        ↓
Image pushed to ECR
        ↓
Jenkins (on EC2) detects new image or is triggered manually
        ↓
Jenkins runs `aws ecs update-service --force-new-deployment`
        ↓
ECS pulls latest image from ECR
        ↓
Container redeploys → App updated automatically 🎉