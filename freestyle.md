# Adding Build Steps in Jenkins

To add a build step in Jenkins, follow these instructions:

1. Click on **"Add build step"**.
2. Choose **"Execute shell"** for Linux/macOS or **"Execute Windows batch command"** for Windows.
 
sh '''
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
'''