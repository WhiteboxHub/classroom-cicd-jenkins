# Check kubernetes context and Deploy

## Jenkins Pipeline Stages

| Stage | Description |
|----------------------------------------------|------------------------------------------------------------|
| **Check Kubernetes Context and Deploy** | Verifies Kubernetes context and displays namespace details. |
| **Deploy to EKS** | Applies Kubernetes deployment and service configurations to EKS. |

### Pipeline Script

```groovy
    stage('Check Kubernetes Context and Deploy') {
        steps {
            withCredentials([file(credentialsId: 'EKS_CONFIG11', variable: 'KUBECONFIG')]) {
                sh 'kubectl cluster-info'
                sh 'kubectl config get-contexts'
                sh 'kubectl config current-context'
                sh 'kubectl get namespaces'
            }
        }
    }

    stage('Deploy to EKS') {
        steps {
            withCredentials([file(credentialsId: 'EKS_CONFIG11', variable: 'KUBECONFIG')]) {
                script {
                    def filesToApply = [
                        "k8s/app-deployment.yaml",
                        "k8s/app-service.yaml",
                        "k8s/configmap.yaml",
                        "k8s/ingress.yaml",
                        "k8s/mongo-deployment.yaml",
                        "k8s/mongo-service.yaml",
                        "k8s/mysql-deployment.yaml",
                        "k8s/mysql-service.yaml",
                        "k8s/redis-deployment.yaml",
                        "k8s/redis-service.yaml",
                        "k8s/secret.yaml"
                    ]
                    for (def file : filesToApply) {
                        echo "Applying ${file}"
                        sh "kubectl apply -f ${file}"
                    }
                }
            }
        }
    }
}
```

## Next Steps

| Step | Description |
|------|-------------|
| **Step 2** | Checking Kubernetes context and verifying namespaces. |
| **Step 3** | Deploying the microservice to Kubernetes (EKS). |

This document will be updated as we progress through each stage of the Jenkins pipeline setup.

