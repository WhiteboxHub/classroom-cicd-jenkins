# Jenkins Pipeline Documentation

## Environment Variables Setup

The following environment variables are required for the Jenkins pipeline:

```groovy
environment {
    microservices_api_repo = "${env.microservices_api_repo}"
    microservices_branch_name = "${env.microservices_branch_name}"
    microservices_docker_image = "${env.microservices_docker_image}"
    KUBECONFIG = "${env.KUBECONFIG}"
    eks_region = "${env.eks_region}"
    ecr_repo_uri = "${env.ecr_repo_uri}"
    DOCKER_HUB_USERNAME = "${env.DOCKER_HUB_USERNAME}"
    DOCKER_HUB_TAGNAME = "${env.DOCKER_HUB_TAGNAME}"
}
```

### Description of Environment Variables

| Variable | Description |
|----------|-------------|
| **microservices_api_repo** | Repository URL of the microservices API. |
| **microservices_branch_name** | The branch from which the code should be pulled. |
| **microservices_docker_image** | Name of the Docker image to be built. |
| **KUBECONFIG** | Path to the Kubernetes configuration file. |
| **eks_region** | AWS region where the EKS cluster is deployed. |
| **ecr_repo_uri** | URI of the ECR repository for storing Docker images. |
| **DOCKER_HUB_USERNAME** | Username for Docker Hub authentication. |
| **DOCKER_HUB_TAGNAME** | Tag name for the Docker image to be pushed. |

## Next Steps

| Step | Description |
|------|-------------|
| **Step 2** | Cloning the repository and checking out the specified branch. |
| **Step 3** | Building and tagging the Docker image. |
| **Step 4** | Pushing the image to ECR or Docker Hub. |
| **Step 5** | Deploying the microservice to Kubernetes. |

This document will be updated as we progress through each stage of the Jenkins pipeline setup.

