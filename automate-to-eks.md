# Automated Deployment to Amazon EKS using Jenkins

## Prerequisites ✅
Ensure you have the following installed and configured:

- **[Docker](https://www.docker.com/get-started)** 🐳 - To build and push container images.
- **[Jenkins](https://www.jenkins.io/download/)** ⚙️ - CI/CD automation tool.
- **[Kubernetes](https://kubernetes.io/docs/setup/)** ☸️ - Container orchestration system.
- **[Git](https://git-scm.com/downloads)** 🛠️ - Version control system.
- **[kubectl](https://kubernetes.io/docs/tasks/tools/)** 📌 - Command-line tool to interact with Kubernetes clusters.
- **[eksctl](https://eksctl.io/introduction/getting-started/)** 🏗️ - CLI tool to create and manage Amazon EKS clusters.
- **[AWS CLI](https://aws.amazon.com/cli/)** ☁️ - CLI to interact with AWS services.


## 🛠 Install Jenkins using Docker

1 **Clone the Jenkins setup repository:**
```sh
 git clone git@github.com:WhiteboxHub/classroom-cicd-jenkins.git
 cd classroom-cicd-jenkins
```

2️ **Build and run Jenkins:**
```sh
 docker build -t my-jenkins .
 docker compose up -d
```

3️ **Access Jenkins UI:**
- Open [http://localhost:8080](http://localhost:8080)
- Retrieve the **initial admin password** from:
```sh
 docker exec my-jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## 1️⃣ Clone the Microservices App
```sh
git clone https://github.com/WhiteboxHub/classroom-api-microservice.git
cd classroom-api-microservice
```

## 2️⃣ Set Up GitHub Webhook for Jenkins
- Go to **GitHub repository → Settings → Webhooks**
- Set **Payload URL** to Jenkins server: `https://your-jenkins-server/github-webhook/`
- Content type: `application/json`
- Select **Just the push event**
- Save the webhook

### Example Webhook Payload
```json
{
  "ref": "refs/heads/main",
  "repository": {
    "clone_url": "https://github.com/user/repo.git"
  },
  "head_commit": {
    "id": "commit_hash"
  }
}
```

## 3️⃣ Environment Variables 🌍
Define the following environment variables in Jenkins:

| Variable | Description |
|----------|-------------|
| `MICROSERVICES_API_REPO` | GitHub repository URL for the microservices app |
| `MICROSERVICES_BRANCH_NAME` | Branch to deploy (e.g., `main`) |
| `MICROSERVICES_DOCKER_IMAGE` | Docker image name for the microservice |
| `KUBECONFIG` | Path to the kubeconfig file |
| `EKS_REGION` | AWS region where EKS is deployed |
| `ECR_REPO_URI` | Amazon ECR repository URI |
| `DOCKER_HUB_USERNAME` | Docker Hub username |
| `DOCKER_HUB_TAGNAME` | Docker image tag |
| `AWS_ACCESS_KEY_ID` | AWS IAM access key |
| `AWS_SECRET_ACCESS_KEY` | AWS IAM secret key |

## 4️⃣ Configure Global Credentials in Jenkins 🔑
| Credential | Usage |
|------------|-------|
| **Git Credentials** | Used for cloning the repository |
| **DockerHub Credentials** | Used to push images to DockerHub |
| **ECR Authentication** | Required to authenticate and push to Amazon ECR |
| **AWS Credentials** | IAM credentials to access AWS |
| **Kubeconfig File** | Enables Jenkins to deploy to Kubernetes |

## 5️⃣ AWS IAM Setup 🔐
### IAM Policy for Jenkins Deployment
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "eks:DescribeCluster",
        "eks:ListClusters",
        "eks:UpdateClusterConfig",
        "ecr:GetAuthorizationToken",
        "ecr:BatchCheckLayerAvailability",
        "ecr:PutImage",
        "ecr:InitiateLayerUpload",
        "ecr:UploadLayerPart",
        "ecr:CompleteLayerUpload",
        "iam:PassRole"
      ],
      "Resource": "*"
    }
  ]
}
```

### Create IAM User
```sh
aws iam create-user --user-name JenkinsUser
```

### Create IAM Group
```sh
aws iam create-group --group-name JenkinsAdmins
aws iam add-user-to-group --user-name JenkinsUser --group-name JenkinsAdmins
```

### Create IAM Role
```sh
echo '{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "ec2.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}' > trust-policy.json
aws iam create-role --role-name JenkinsRole --assume-role-policy-document file://trust-policy.json
```

### Attach IAM Policy
```sh
aws iam attach-user-policy --user-name JenkinsUser --policy-arn arn:aws:iam::<AWS_ACCOUNT_ID>:policy/JenkinsPolicy
```

## 6️⃣ Create an Amazon ECR Repository 🏗️
```sh
aws ecr create-repository --repository-name microservices-fastapi
```

## 7️⃣ Deploy to Kubernetes on AWS EKS ☸️
### Create an EKS Cluster
```sh
eksctl create cluster --name microservices-cluster --region ${EKS_REGION} --nodegroup-name workers --node-type t3.medium --nodes 2
```

### Verify Kubernetes Contexts
```sh
kubectl config get-contexts
kubectl config use-context <context-name>
```

### Deploy Microservices to EKS
```sh
kubectl apply -f k8s/app-deployent.yaml
kubectl apply -f k8s/app-service.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/ingress.yaml
kubectl apply -f k8s/mongo-deployment.yaml
kubectl apply -f k8s/app-service.yaml
kubectl apply -f k8s/mysql-deployment.yaml
kubectl apply -f k8s/mysql-service.yaml
kubectl apply -f k8s/redis-deployment.yaml
kubectl apply -f k8s/redis-service.yaml
kubectl apply -f secret.yaml

```

## Verify Deployment

### 1. Check if all Pods are Running

Run the following command to verify that all pods are in a `Running` state:

```sh
kubectl get pods fastapi-app-7d4d85dc8c-fx8hl
```

If any pod is in `CrashLoopBackOff` or `Pending` state, check logs and describe the pod for debugging:

```sh
kubectl logs 
kubectl describe pod fastapi-app-7d4d85dc8c-fx8hl
```

### 2. Retrieve the Application's Public URL

#### If Using LoadBalancer Service

Check the service's external IP:

```sh
kubectl get svc
```

Look for the `EXTERNAL-IP` column under the service exposing your application. If it's pending, wait a few minutes and retry.

#### If Using Ingress

Check the public Ingress URL:

```sh
kubectl get ingress
```

The output will include the public hostname or IP address.

### 3. Access the Application

Once you have the public IP or hostname, open it in your browser:

```sh
http://<EXTERNAL-IP> or http://<INGRESS-HOSTNAME>
```

## Troubleshooting

- Ensure your security groups and IAM roles allow proper access.
- Check `kubectl describe` output for misconfigurations.
- Verify AWS LoadBalancer health checks and target groups.
- Ensure the Ingress controller is properly installed and configured.

---

This README provides a structured approach to deploying and verifying your microservices on AWS EKS. 🚀


## Contributors
- whitebox learning