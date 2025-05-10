# Jenkins Pipeline for Kubernetes Deployment

This Jenkins pipeline automates the process of checking out Kubernetes manifests, updating the Minikube context, verifying cluster information, and deploying applications to Minikube.

## Prerequisites
- Jenkins installed and configured.
- Minikube installed on the Jenkins agent.
- Kubernetes CLI (`kubectl`) installed.
- Git repository containing Kubernetes manifests.

## Pipeline Stages

### 1. Check Minikube
This stage verifies if Minikube is installed and running.

```groovy
stage('Check Minikube') {
    steps {
        sh '''
            which minikube
            minikube version
            minikube status
        '''
    }
}
```

### 2. Start Minikube
This stage starts Minikube if it is not already running and enables the Ingress addon.

```groovy
stage('Start Minikube') {
    steps {
        script {
            def minikubeStatus = sh(script: 'minikube status || echo "Stopped"', returnStdout: true).trim()
            if (!minikubeStatus.contains("Running")) {
                sh '''
                    minikube start --driver=docker
                    minikube addons enable ingress
                '''
            } else {
                echo "Minikube is already running."
            }
        }
    }
}
```

### 3. Checkout Kubernetes Manifests
This stage clones the Kubernetes manifests from the specified repository branch.

```groovy
stage('Checkout Kubernetes Manifests') {
    steps {
        git branch: "${MANIFESTO_BRANCH_NAME}",
            url: "${K8S_REPO_URL}"
    }
}
```

### 4. Update Minikube Context
This stage updates the Minikube context.

```groovy
stage('Update Minikube Context') {
    steps {
        sh '''
            export KUBECONFIG="C:/Users/your_folder/.kube/config"
            minikube update-context
        '''
    }
}
```

### 5. Check Cluster Info
This stage retrieves information about the Kubernetes cluster.

```groovy
stage('Check Cluster Info') {
    steps {
        sh '''
            export KUBECONFIG="C:/Users/your_folder/.kube/config"
            kubectl cluster-info
        '''
    }
}
```

### 6. Deploy to Minikube
This stage applies Kubernetes configurations to deploy the application.

```groovy
stage('Deploy to Minikube') {
    steps {
        sh '''
            kubectl apply -f configmap.yaml
            kubectl apply -f deployment.yaml
            kubectl apply -f service.yaml
            kubectl apply -f ingress.yaml
        '''
    }
}
```

### 7. Verify Deployment
This stage verifies the status of the Kubernetes deployment.

```groovy
stage('Verify Deployment') {
    steps {
        sh '''
            echo "Checking pods..."
            kubectl get pods -o wide
            echo "Checking services..."
            kubectl get svc
            echo "Checking ingress..."
            kubectl get ingress
        '''
    }
}
```

## Environment Variables
| Variable | Description |
|----------|-------------|
| `MANIFESTO_BRANCH_NAME` | The Git branch to checkout Kubernetes manifests |
| `K8S_REPO_URL` | The URL of the Git repository containing Kubernetes manifests |

## Usage
1. Configure Jenkins with the necessary credentials and Minikube setup.
2. Set up the pipeline using the above script.
3. Trigger the build to deploy the Kubernetes manifests.


