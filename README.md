# 🚀 Jenkins CI/CD Pipeline Setup

## 📌 Overview
This guide walks you through setting up **Jenkins** for a complete CI/CD pipeline, integrating **GitHub, Docker, and Kubernetes**. Follow the steps to configure Jenkins, set up credentials, and deploy your application to **Minikube or Kubernetes clusters**.

---

## ⚙️ Prerequisites

Ensure you have the following installed:
- **Docker** (Install [here](https://docs.docker.com/get-docker/))
- **Jenkins** (via Docker or native installation)
- **Git** (Install [here](https://git-scm.com/downloads))
- **Kubectl** (Install [here](https://kubernetes.io/docs/tasks/tools/install-kubectl/))
- **Minikube** (Install [here](https://minikube.sigs.k8s.io/docs/start/))

---

## 🛠 Install Jenkins using Docker

1️⃣ **Clone the Jenkins setup repository:**
```sh
 git clone git@github.com:WhiteboxHub/classroom-cicd-jenkins.git
 cd classroom-cicd-jenkins
```

2️⃣ **Build and run Jenkins:**
```sh
 docker build -t my-jenkins .
 docker compose up -d
```

3️⃣ **Access Jenkins UI:**
- Open [http://localhost:8080](http://localhost:8080)
- Retrieve the **initial admin password** from:
```sh
 docker exec my-jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

4️⃣ **Install suggested plugins and create an admin user.**

---

## 🔐 Configuring Jenkins Credentials

After setting up Jenkins, navigate to **Manage Jenkins → Manage Credentials** and add the following:

| Credential ID | Type | Used For |
|--------------|------|----------|
| `github-credentials` | Username & Password | Accessing private GitHub repositories |
| `docker-hub-credentials` | Username & Password | Pushing images to Docker Hub |
| `kubeconfig` | Secret File | Accessing Kubernetes cluster |

---

## 📄 Defining Environment Variables

Add these **global environment variables** in **Manage Jenkins → Configure System**:

| Variable | Purpose |
|----------|---------|
| `KUBECONFIG` | Path to Kubernetes config file |
| `BRANCH_NAME` | Branch for the API repo (`main` or `dev`) |
| `REPO_URL` | URL of the **API repo** (`github.com:WhiteboxHub/classroom-api-simple.git`) |
| `K8S_REPO_URL` | URL of the **Kubernetes repo** (`https://github.com/WhiteboxHub/classroom-cicd-kubernetes.git`) |
| `K8S_REPO_BRANCH_NAME` | Branch of the Kubernetes repo (`main`) |
| `DOCKER_HUB_USERNAME` | Docker Hub username |
| `DOCKER_HUB_TAGNAME` | Docker image tag |
| `DOCKER_IMAGE` | Docker image name (`my-api-image`) |
| `CONTAINER_NAME` | Name of the running container |
| `MANIFESTO_BRANCH_NAME` | Kubernetes deployment branch |
| `K8S_REPO_FOLDER_NAME` | Directory where Kubernetes manifests are stored |

---

## 🔄 Setting Up Webhook for Auto-Trigger

To enable automatic builds on code changes:

1️⃣ **Go to GitHub repository settings** (`classroom-api-simple.git`).
2️⃣ **Navigate to Webhooks → Add webhook**.
3️⃣ **Enter the Jenkins URL:**
```
http://your-jenkins-url/github-webhook/
```
4️⃣ **Set content type** to `application/json`.
5️⃣ **Save changes**.

🔹 This ensures that when you push changes to GitHub, Jenkins triggers the pipeline automatically.

---

## 🚀 Running the CI/CD Pipeline

1️⃣ **Go to Jenkins → Manage Jenkins → New Item → Pipeline.**
2️⃣ **Enter the repository URL:**
```
https://github.com/WhiteboxHub/classroom-api-simple.git
```
3️⃣ **Select "Pipeline script from SCM" and choose `Jenkinsfile`.**
4️⃣ **Save and run the build.**

✅ **Your application is now built, containerized, and deployed to Kubernetes!** 🎉

---

## 📢 Verifying Deployment in Kubernetes

Check running pods:
```sh
kubectl get pods
```

View logs of a pod:
```sh
kubectl logs <pod-name>
```

Check service endpoints:
```sh
kubectl get svc
```

---

## 🧹 Cleanup
To stop Jenkins:
```sh
 docker compose down
```

To remove all Docker volumes and images:
```sh
 docker compose down --volumes --rmi all
```

To delete Kubernetes deployments:
```sh
kubectl delete -f deployment.yaml
```

---

## 🎯 Contributors
- **whitebox learning** 



