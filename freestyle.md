# Jenkins Freestyle Build Steps

This guide provides instructions on how to add build steps in a Jenkins freestyle project without integrating with SCM.

## Steps to Add a Build Step in Jenkins

1. Open Jenkins and create a new **Freestyle Project**.
2. Scroll down to the **Build** section.
3. Click on **Add build step**.
4. Choose **Execute shell** (for Linux/macOS) or **Execute Windows batch command** (for Windows).
5. Enter the following script in the command section:

### Shell Script (windows/Linux/macOS)
```sh
#!/bin/bash

echo "---------------------------------"
echo "Jenkins Freestyle CI/CD Pipeline"
echo "---------------------------------"

# Step 1: Build
echo "[Build] Compiling the code..."
sleep 2
echo "[Build] Compilation Successful!"

# Step 2: Test
echo "[Test] Running automated tests..."
sleep 2
echo "[Test] All tests passed!"

# Step 3: Package
echo "[Package] Creating a deployment artifact..."
sleep 2
touch app.tar.gz
echo "[Package] Artifact created: app.tar.gz"

# Step 4: Deploy
echo "[Deploy] Simulating deployment..."
sleep 2
echo "[Deploy] Application successfully deployed!"

echo "---------------------------------"
echo "CI/CD Process Completed!"
echo "---------------------------------"

exit 0
```


6. Click **Save** and then **Build Now** to test the process.

This will simulate a complete CI/CD pipeline, from building to deploying an application, without requiring SCM integration.

