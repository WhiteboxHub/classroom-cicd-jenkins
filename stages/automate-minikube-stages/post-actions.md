# Jenkins stage for Post Deployment status


###  Post Deployment Status
This stage logs whether the deployment was successful or failed.

```groovy
post {
    success {
        echo 'Deployment success'
    }
    failure {
        echo 'Deployment failure'
    }
}
```

