sh ''' 
environment {
    KUBECONFIG = "${env.KUBECONFIG}"
    BRANCH_NAME = "${env.BRANCH_NAME}"
    REPO_URL = "${env.REPO_URL}"
    K8S_REPO_URL = "${env.K8S_REPO_URL}"
    K8S_REPO_BRANCH_NAME = "${env.K8S_REPO_BRANCH_NAME}"
    DOCKER_HUB_USERNAME = "${env.DOCKER_HUB_USERNAME}"
    DOCKER_HUB_TAGNAME = "${env.DOCKER_HUB_TAGNAME}"
    DOCKER_IMAGE = "${env.DOCKER_IMAGE}"
    CONTAINER_NAME = "${env.CONTAINER_NAME}"
    MANIFESTO_BRANCH_NAME = "${env.MANIFESTO_BRANCH_NAME}"
    K8S_REPO_FOLDER_NAME = "${env.K8S_REPO_FOLDER_NAME}"
}
''' 
# Start Generation Here
sh '''
# 📄 Configuring Environment Variables in Jenkins

| Variable                     | Purpose                                                  |
|------------------------------|----------------------------------------------------------|
| `KUBECONFIG`                 | Path to Kubernetes config file                           |
| `BRANCH_NAME`                | Branch for the API repository (`main` or `dev`)        |
| `REPO_URL`                   | URL of the API repository (`github.com:WhiteboxHub/classroom-api-simple.git`) |
| `K8S_REPO_URL`               | URL of the Kubernetes repository (`https://github.com/WhiteboxHub/classroom-cicd-kubernetes.git`) |
| `K8S_REPO_BRANCH_NAME`       | Branch of the Kubernetes repository (`main`)            |
| `DOCKER_HUB_USERNAME`        | Docker Hub username                                      |
| `DOCKER_HUB_TAGNAME`         | Docker image tag                                         |
| `DOCKER_IMAGE`               | Docker image name (`my-api-image`)                      |
| `CONTAINER_NAME`             | Name of the running container                            |
| `MANIFESTO_BRANCH_NAME`      | Kubernetes deployment branch                             |
| `K8S_REPO_FOLDER_NAME`       | Directory where Kubernetes manifests are stored         |

## Steps to Configure Environment Variables

1. **Access Jenkins Dashboard**: Open your Jenkins instance in a web browser.

2. **Navigate to Configure System**: Go to **Manage Jenkins** > **Configure System**.

3. **Add Environment Variables**: Scroll down to the **Global properties** section and check the box for **Environment variables**. Then, add the variables listed in the table above.

4. **Save Changes**: After adding all the required variables, scroll down and click on the **Save** button to apply the changes.

5. **Verify Configuration**: To ensure that the environment variables are set correctly, you can create a simple pipeline job that echoes the variables to the console output.

'''
# End Generation Here
