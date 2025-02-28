# CI/CD Pipeline Setup with Jenkins, Docker, and AWS EC2

## Prerequisites
Ensure the following tools are installed before proceeding:
- Docker
- Kubectl
- Git
- Jenkins
- AWS CLI

## Step 1: Set Up a Simple API Application
1. Clone the sample API application repository:
   ```sh
   git clone https://github.com/WhiteboxHub/classroom-cicd-jenkins.git
   cd classroom-cicd-jenkins
   ```
2. Verify that a `Jenkinsfile` exists in the project directory.

## Step 2: Jenkins Setup
### Install Required Plugins
1. Open Jenkins and navigate to **Manage Jenkins > Plugin Manager**.
2. Install the following plugins:
   - Pipeline
   - Git
   - Docker Pipeline
   - SSH Pipeline Steps
   - AWS CLI

### Configure Environment Variables and Credentials
1. Go to **Manage Jenkins > Configure System** and add the following environment variables:
   | Variable | Description |
   |----------|-------------|
   | EC2_USER | EC2 instance username |
   | EC2_HOST | EC2 instance IP or hostname |
   | DOCKERHUB_EC2_REPOSITORY_NAME | DockerHub repository name |
   | DOCKERHUB_EC2_DOCKER_TAGNAME | Docker image tag |
   | DOCKER_EC2_IMAGE_NAME | Docker image name |
   | DOCKER_CONTAINER_NAME_EC2 | Docker container name |
   | APP_PORT | Application port |
   | GIT_EC2_BRANCH_NAME | Git branch for deployment |
   | GIT_EC2_REPO_NAME | Git repository URL |

2. Go to **Manage Jenkins > Manage Credentials** and add the following credentials:
   - **DockerHub Credentials:** Username and password for DockerHub.
   - **EC2 SSH Key:** Private SSH key for connecting to EC2.
   - **Ngrok Token:** Authentication token for Ngrok.

### Add Remote Repository
1. Navigate to **Jenkins Dashboard > New Item > Pipeline**.
2. Select **Pipeline** and click **OK**.
3. In **Pipeline Configuration**, set the repository URL to the API repository.
4. Ensure the **Jenkinsfile** is present in the repository.

### Configure Ngrok for Webhooks
1. Install and configure Ngrok:
   ```sh
   ngrok http 8080
   ```
2. Copy the generated public URL and use it for GitHub webhooks.
3. Add a webhook in GitHub: **Settings > Webhooks > Add Webhook**.
   - Payload URL: `<NGROK_PUBLIC_URL>/github-webhook/`
   - Content Type: `application/json`
   - Trigger: Push events

## Step 3: EC2 Instance Setup
### Create and Connect to EC2 Instance
1. Create an EC2 instance:
   ```sh
   aws ec2 run-instances --image-id ami-xxxxxxx --count 1 --instance-type t2.micro --key-name MyKeyPair --security-groups MySecurityGroup
   ```
2. Download the key pair and connect:
   ```sh
   chmod 400 MyKeyPair.pem
   ssh -i MyKeyPair.pem ec2-user@<EC2_INSTANCE_IP>
   ```

### Install Required Packages on EC2
1. Update and install Docker:
   ```sh
   sudo yum update -y
   sudo yum install docker -y
   sudo systemctl start docker
   sudo systemctl enable docker
   ```
2. Install Kubernetes tools:
   ```sh
   curl -LO "https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl"
   chmod +x ./kubectl
   sudo mv ./kubectl /usr/local/bin/kubectl
   ```
3. Install Ingress:
   ```sh
   kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/aws/deploy.yaml
   ```

### Deploy Docker Image to EC2
1. Pull and run the Docker image:
   ```sh
   docker pull <DOCKERHUB_EC2_REPOSITORY_NAME>/<DOCKERHUB_EC2_DOCKER_TAGNAME>:<DOCKER_EC2_IMAGE_NAME>
   docker run -d -p <APP_PORT>:<APP_PORT> --name <DOCKER_CONTAINER_NAME_EC2> <DOCKERHUB_EC2_REPOSITORY_NAME>/<DOCKERHUB_EC2_DOCKER_TAGNAME>:<DOCKER_EC2_IMAGE_NAME>
   ```

## Contributors
- **WhiteBox Learning**.

---

This guide ensures a smooth CI/CD pipeline setup for automated deployments from GitHub to AWS EC2 using Jenkins, Docker, and Kubernetes.

