# Jenkins Setup Using Docker

## Prerequisites
Ensure you have the following installed on your system:
- [Docker](https://docs.docker.com/get-docker/)

## Installation Steps

### 1️⃣ Clone the Jenkins Setup Repository
```sh
 git clone https://github.com/WhiteboxHub/classroom-cicd-jenkins.git
 cd classroom-cicd-jenkins
```

### 2️⃣ Build and Run Jenkins
```sh
 docker build -t my-jenkins .
 docker compose up -d
```

### 3️⃣ Access Jenkins UI
- Open [http://localhost:8080](http://localhost:8080) in your browser.
- Retrieve the **initial admin password** by running:
```sh
 docker exec my-jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

### 4️⃣ Install Suggested Plugins and Create an Admin User
Follow the on-screen instructions to install the recommended plugins and create an administrator account.

---
Now, your Jenkins setup is complete and ready to use!

## Contributors
- Whitebox Learning

