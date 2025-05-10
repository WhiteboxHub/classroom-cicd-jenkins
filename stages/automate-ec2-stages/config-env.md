# EC2 Deployment Environment Variables

This document provides an overview of the environment variables required for setting up and deploying an application on an EC2 instance.

## Environment Variables
The following environment variables need to be configured:

```sh
environment {
    EC2_USER = "${env.EC2_USER}"
    EC2_HOST = "${env.EC2_HOST}"
    DOCKERHUB_EC2_REPOSITORY_NAME = "${env.DOCKERHUB_EC2_REPOSITORY_NAME}"
    DOCKERHUB_EC2_DOCKER_TAGNAME = "${env.DOCKERHUB_EC2_DOCKER_TAGNAME}"
    DOCKER_EC2_IMAGE_NAME = "${env.DOCKER_EC2_IMAGE_NAME}"
    DOCKER_CONTAINER_NAME_EC2 = "${env.DOCKER_CONTAINER_NAME_EC2}"
    APP_PORT = "${env.APP_PORT}"
    GIT_EC2_BRANCH_NAME = "${env.GIT_EC2_BRANCH_NAME}"
    GIT_EC2_REPO_NAME = "${env.GIT_EC2_REPO_NAME}"
}
```

## Configuration Steps
Follow these steps to configure the environment variables:

1. **Edit your configuration file**: Add the above environment block to your configuration file (e.g., `.env` or a deployment script).
2. **Export environment variables**: If configuring manually, run the following commands in your terminal:
   ```sh
   export EC2_USER="your-ec2-user"
   export EC2_HOST="your-ec2-instance-ip"
   export DOCKERHUB_EC2_REPOSITORY_NAME="your-docker-repo"
   export DOCKERHUB_EC2_DOCKER_TAGNAME="latest"
   export DOCKER_EC2_IMAGE_NAME="your-image-name"
   export DOCKER_CONTAINER_NAME_EC2="your-container-name"
   export APP_PORT="3000"
   export GIT_EC2_BRANCH_NAME="main"
   export GIT_EC2_REPO_NAME="your-repo-name"
   ```
3. **Verify the variables**: Run `env | grep EC2_` to confirm the variables are correctly set.
4. **Deploy the application**: Proceed with deploying your application using Docker and Git.

## Environment Variables Table

| Variable Name                       | Description |
|--------------------------------------|-------------|
| `EC2_USER`                           | The username for accessing the EC2 instance. |
| `EC2_HOST`                           | The public IP or DNS of the EC2 instance. |
| `DOCKERHUB_EC2_REPOSITORY_NAME`      | The DockerHub repository name for storing the application image. |
| `DOCKERHUB_EC2_DOCKER_TAGNAME`       | The tag name used for the Docker image in DockerHub. |
| `DOCKER_EC2_IMAGE_NAME`              | The name of the Docker image that will be used on EC2. |
| `DOCKER_CONTAINER_NAME_EC2`          | The name of the Docker container running on EC2. |
| `APP_PORT`                           | The application port used for running the service. |
| `GIT_EC2_BRANCH_NAME`                | The Git branch name to be used for deployment. |
| `GIT_EC2_REPO_NAME`                  | The name of the Git repository containing the source code. |

## Next Steps
Once these variables are set, you can proceed with deploying your application on the EC2 instance using Docker and Git.