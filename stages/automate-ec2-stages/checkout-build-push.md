# EC2 Deployment Stages

This document outlines the stages involved in deploying an application on an EC2 instance using Jenkins pipelines.

## Deployment Stages

### 1. Checkout the Code
```groovy
stage('Checkout the Code') {
    steps {
        git branch: "${GIT_EC2_BRANCH_NAME}", url: "${GIT_EC2_REPO_NAME}"
    }
}
```
- Pulls the source code from the specified Git repository and branch.

### 2. Build Docker Image
```groovy
stage('Build Docker Image') {
    steps {
        sh '''
            docker build -t ${DOCKER_EC2_IMAGE_NAME} .
            docker tag ${DOCKER_EC2_IMAGE_NAME} ${DOCKERHUB_EC2_REPOSITORY_NAME}/${DOCKERHUB_EC2_DOCKER_TAGNAME}:${DOCKER_EC2_IMAGE_NAME}
        '''
    }
}
```
- Builds the Docker image using the Dockerfile.
- Tags the image with the appropriate repository and tag name.

### 3. Push Docker Image to Docker Hub
```groovy
stage('Push Docker Image to Docker Hub') {
    steps {
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'DOCKER_HUB_PASS')]) {
            sh '''
                echo $DOCKER_HUB_PASS | docker login -u ${DOCKERHUB_EC2_REPOSITORY_NAME} --password-stdin
                docker push ${DOCKERHUB_EC2_REPOSITORY_NAME}/${DOCKERHUB_EC2_DOCKER_TAGNAME}:${DOCKER_EC2_IMAGE_NAME}
            '''
        }
    }
}
```
- Logs into Docker Hub using stored credentials.
- Pushes the built Docker image to the specified repository.

## Summary Table

| Stage | Description |
|--------|-------------|
| Checkout the Code | Clones the repository and checks out the specified branch. |
| Build Docker Image | Builds a Docker image and tags it appropriately. |
| Push Docker Image to Docker Hub | Logs into Docker Hub and pushes the built image. |

## Next Steps
After pushing the Docker image, you can proceed with deploying it on an EC2 instance.

