# Build and Push to Docker Registry

## Jenkins Pipeline Stages

| Stage | Description |
|-------|-------------|
| **Docker Build** | Builds the Docker image, tags it, and pushes it to Docker Hub. |

### Pipeline Script

```groovy
    
    stage('Docker Build') {
        steps {
            sh '''
                echo "Building Docker image ...."
                docker build -t ${microservices_docker_image} .
                docker tag ${microservices_docker_image} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_TAGNAME}:${microservices_docker_image}
                echo "Pushing Docker image to Docker Hub ...."
                docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_TAGNAME}:${microservices_docker_image}
            '''
        }
    }
```

## Next Steps

| Step | Description |
|------|-------------|
| **Step 2** | Building, tagging, and pushing the Docker image. |
| **Step 3** | Deploying the microservice to Kubernetes. |

This document will be updated as we progress through each stage of the Jenkins pipeline setup.

