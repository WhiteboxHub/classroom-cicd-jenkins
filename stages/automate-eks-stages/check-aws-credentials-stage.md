# Check AWS credentials 

## Jenkins Pipeline Stages

| Stage | Description |
|--------------------------------|------------------------------------------------------------|
| **Check AWS Credentials** | Verifies AWS credentials and retrieves caller identity. |
| **Push to DockerHub/ECR** | Logs into Docker Hub/ECR and pushes the Docker image. |

### Pipeline Script

```groovy
stages {
    stage('Check AWS Credentials') {
        steps {
            script {
                echo "AWS_ACCESS_KEY_ID: ${env.AWS_ACCESS_KEY_ID}"
                echo "AWS_SECRET_ACCESS_KEY: ${env.AWS_SECRET_ACCESS_KEY}"
                sh 'aws sts get-caller-identity'
            }
        }
    }
    
    stage('Push to DockerHub/ECR') {
        steps {
            withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'DOCKER_HUB_PASS')]) {
                sh '''
                    echo "$DOCKER_HUB_PASS" | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin
                    docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_TAGNAME}:${microservices_docker_image}
                '''
            }
        }
    }
}
```

## Next Steps

| Step | Description |
|------|-------------|
| **Step 2** | Building, tagging, and pushing the Docker image. |
| **Step 3** | Checking AWS credentials before deployment. |

This document will be updated as we progress through each stage of the Jenkins pipeline setup.

