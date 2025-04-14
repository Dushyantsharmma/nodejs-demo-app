#   TASK 1 Automate Code Deployment Using CICD Pipeline
---
**Overview**
  Automatically build, test, and deploy your application every time you push code to your repository.

 **Tools Involved**
  - GitHub Actions: Automate your build, test, and deployment steps.

  - Docker: Package your app into a container.

  - DockerHub: Store and share your container images.

  - Node.js: (app.js) – the application being deployed.

# What Happens in the Pipeline?
   # Trigger:

       - The pipeline starts automatically on a git push to the main branch.

   # Checkout Code:

       - Pulls your source code into the GitHub Actions runner.

  # Install Dependencies

      - Runs npm install to set up the app.

  # Run Tests

      - Runs npm test to ensure everything is working.

  # Build Docker Image

      - Builds a Docker image from the Dockerfile.

  # Login to DockerHub

      - Uses your DockerHub credentials stored in GitHub Secrets.

  # Push Image to DockerHub

      - Pushes the image to your DockerHub repository for deployment or sharing.

**Prerequisites:**
   # Before you begin:

        - Have a GitHub account

        - Have a DockerHub account

        - Create a GitHub repo for your Node.js app

        - Generate a DockerHub access token: https://hub.docker.com/settings/security



**File Structure:**

  ![Image](https://github.com/user-attachments/assets/d8cdebed-c371-4719-8121-e65ee309cdb5)

 **GitHub Secrets:**
   - Go to your GitHub repo → Settings → Secrets and Variables → Actions -> Repository secrets

**Add:**

   - DOCKER_USERNAME = your DockerHub username   # give here docker hub username          (imp: same name DOCKER_USERNAME given in inside of main.yml                 
     .github/workflows/main.yml)

  - DOCKER_PASSWORD = your DockerHub access token   # give here docker hub               PAT(personal access token) (imp: same name                DOCKER_PASSWORD given in inside of        main.yml .github/workflows/main.yml)

# GitHub Actions CI/CD: .github/workflows/main.yml
```sh
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  workflow_dispatch: # this is use for enable run pipeline Button

jobs:
  build-test-push:
    name: Build, Test & Push Docker Image
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install Dependencies
        run: npm install

      - name: Run Tests
        run: npm test

      - name: Log in to DockerHub
        uses: docker/login-action@v3
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build Docker Image
        run: |
          docker build -t ${{ secrets.DOCKER_USERNAME }}/nodejs-demo-app:latest .

      - name: Push Docker Image to DockerHub
        run: |
          docker push ${{ secrets.DOCKER_USERNAME }}/nodejs-demo-app:latest
```
![Image](https://github.com/user-attachments/assets/ea0f9921-5695-4ae3-8592-1075051316bd)

![Image](https://github.com/user-attachments/assets/f26c0987-9260-4257-a557-48311cb11b1e)

**Goals:** Automatically build, test, and deploy your application every time you push code to your repository.


      
