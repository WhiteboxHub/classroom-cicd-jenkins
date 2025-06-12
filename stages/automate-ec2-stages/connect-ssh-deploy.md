# EC2 Deployment via SSH

This document provides an overview of the SSH setup and deployment process for deploying an application on an EC2 instance using Jenkins.

## Deployment Stages

### Check SSH Connection

```groovy
stage("Check SSH") {
    steps {
        script {
            echo "SSH Key: ${SSH_KEY}"
        }
    }
}
```

This stage verifies that the SSH key is correctly loaded.

### Deploy on EC2

```groovy
stage('Deploy on EC2') {
    steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'EC2_SSH_KEY', keyFileVariable: 'SSH_KEY', passphraseVariable: '', usernameVariable: 'SSH_USER')]) {
            sh '''
                # Log SSH key path
                echo "Using SSH key at: ${SSH_KEY}"

                # Ensure correct permissions for the SSH key
                chmod 600 ${SSH_KEY}

                # Debug SSH connection before deployment
                ssh -vvv -i ${SSH_KEY} -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} "whoami"

                # Deploy the application
                ssh -i ${SSH_KEY} ${EC2_USER}@${EC2_HOST} "
                    mkdir -p ${EC2_DIR};
                    docker pull ${DOCKERHUB_EC2_REPOSITORY_NAME}/${DOCKERHUB_EC2_DOCKER_TAGNAME}:${DOCKER_EC2_IMAGE_NAME};
                    docker stop ${DOCKER_CONTAINER_NAME_EC2} || true;
                    docker rm ${DOCKER_CONTAINER_NAME_EC2} || true;
                    docker run -d -p ${APP_PORT}:${APP_PORT} --name ${DOCKER_CONTAINER_NAME_EC2} ${DOCKERHUB_EC2_REPOSITORY_NAME}/${DOCKERHUB_EC2_DOCKER_TAGNAME}:${DOCKER_EC2_IMAGE_NAME}
                "
            '''
        }
    }
}
```

## Configuration Steps

1. **Ensure SSH Key is configured**
   - Add your SSH key to Jenkins credentials with ID `EC2_SSH_KEY`.
   - Verify that the key has correct permissions (`chmod 600`).

2. **Verify SSH Connection**
   - The `Check SSH` stage will print the SSH key path to confirm its presence.
   - The `ssh -vvv` command helps debug any connection issues.

3. **Deploy the Application**
   - The deployment process involves:
     - Creating the necessary directory on EC2.
     - Pulling the latest Docker image from DockerHub.
     - Stopping and removing any existing containers.
     - Running the new container on the specified port.

## Next Steps
After successful deployment, verify the running container using:
```sh
ssh -i <your-ssh-key.pem> <EC2_USER>@<EC2_HOST> "docker ps"
```

