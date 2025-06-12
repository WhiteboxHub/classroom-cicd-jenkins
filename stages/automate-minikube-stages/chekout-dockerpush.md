# Jenkins Pipeline for Docker Image Build and Push

This Jenkins pipeline automates the process of building a Docker image, tagging it, and pushing it to Docker Hub.

## Prerequisites
- Jenkins installed and configured.
- Docker installed on the Jenkins agent.
- Credentials for Docker Hub stored in Jenkins as `DOCKER_HUB_PASSWORD`.
- A Git repository containing the application code.

## Pipeline Stages

### 1. Checkout Code
This stage clones the code from the specified repository branch.

```groovy
stage('Checkout Code') {
    steps {
        git branch: "${env.BRANCH_NAME}",
            url: "${env.REPO_URL}"
    }
}
```

### 2. Build Docker Image
This stage builds the Docker image and tags it appropriately.

```groovy
stage('Build Docker Image') {
    steps {
        sh '''
            docker build -t ${DOCKER_IMAGE} .
            docker tag ${DOCKER_IMAGE} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_TAGNAME}:${DOCKER_IMAGE}
        '''
    }
}
```

### 3. Push Docker Image to Docker Hub
This stage logs into Docker Hub and pushes the image.

```groovy
stage('Push Docker Image to Docker Hub') {
    steps {
        withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'DOCKER_HUB_PASS')]) {
            sh '''
                echo "$DOCKER_HUB_PASS" | docker login -u ${DOCKER_HUB_USERNAME} --password-stdin
                docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_TAGNAME}:${DOCKER_IMAGE}
            '''
        }
    }
}
```

## Environment Variables
| Variable | Description |
|----------|-------------|
| `BRANCH_NAME` | The Git branch to checkout |
| `REPO_URL` | The URL of the Git repository |
| `DOCKER_IMAGE` | Name of the Docker image |
| `DOCKER_HUB_USERNAME` | Docker Hub username |
| `DOCKER_HUB_TAGNAME` | Tag name for Docker image |

## Usage
1. Configure Jenkins with the necessary credentials.
2. Set up the pipeline using the above script.
3. Trigger the build to build and push the Docker image.


