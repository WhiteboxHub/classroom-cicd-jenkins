# Jenkins Pipeline for Kubernetes Deployment

This Jenkins pipeline automates the process of checking out Kubernetes manifests, updating the Minikube context, and verifying cluster information.

## Prerequisites
- Jenkins installed and configured.
- Minikube installed on the Jenkins agent.
- Kubernetes CLI (`kubectl`) installed.
- Git repository containing Kubernetes manifests.

## Pipeline Stages

### 1. Checkout Kubernetes Manifests
This stage clones the Kubernetes manifests from the specified repository branch.

```groovy
stage('Checkout Kubernetes Manifests') {
    steps {
        git branch: "${MANIFESTO_BRANCH_NAME}",
            url: "${K8S_REPO_URL}"
    }
}
```

### 2. Update Minikube Context
This stage updates the Minikube context.

```groovy
stage('Update Minikube Context') {
    steps {
        sh '''
            export KUBECONFIG="C:/Users/dhira/.kube/config"
            minikube update-context
        '''
    }
}
```

### 3. Check Cluster Info
This stage retrieves information about the Kubernetes cluster.

```groovy
stage('Check Cluster Info') {
    steps {
        sh '''
            export KUBECONFIG="C:/Users/dhira/.kube/config"
            kubectl cluster-info
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


