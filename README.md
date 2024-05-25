# Django Notes Application CI/CD Pipeline

This repository contains a Django notes application integrated with a Jenkins Declarative CI/CD pipeline. The pipeline automates the deployment process using GitHub webhooks, Docker, and Docker Compose. This setup ensures efficient and reliable deployment of the application.

## Project Overview

This project demonstrates the following key aspects:

- **CI/CD Pipeline with Jenkins**: Automates the build, test, and deployment process.
- **GitHub Integration**: Uses webhooks to trigger the pipeline on code commits.
- **Docker**: Containers the application for consistency across different environments.
- **Docker Compose**: Manages multi-container deployment.

## Pipeline Stages

The CI/CD pipeline includes the following stages:

1. **Clone the Code from GitHub**: 
   - Jenkins pulls the latest code from the GitHub repository.
2. **Build the Docker Image**: 
   - The Docker image for the Django application is built.
3. **Push to Docker Hub**: 
   - The Docker image is tagged and pushed to Docker Hub.
4. **Deploy Using Docker Compose**: 
   - The application is deployed using Docker Compose.

## Jenkinsfile

Here is the `Jenkinsfile` used for the pipeline:

```groovy
pipeline {
    agent any
    
    environment {
        DOCKER_API_VERSION = '1.41'
    }
    
    stages {
        stage("Clone the Code") {
            steps {
                echo "Cloning the code from GitHub..."
                git url: "https://github.com/kevalhingu/notes-app-django.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the Docker image..."
                sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: "DockerHub", passwordVariable: "DockerHubPass", usernameVariable: "DockerHubUser")]) {
                    sh "docker tag my-note-app ${env.DockerHubUser}/my-note-app:latest"
                    sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                    sh "docker push ${env.DockerHubUser}/my-note-app:latest"
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the application..."
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
```
## YouTube Tutorial:

For a detailed, step-by-step guide, watch the tutorial on YouTube:

[Watch the Tutorial](https://youtu.be/vX40n8B-_oo?si=m8sZKyWojS0NpSW7)

## Resources:
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Docker Documentation](https://docs.docker.com/)
- [GitHub Webhooks Guide](https://docs.github.com/en/developers/webhooks-and-events/webhooks)
- [Docker Hub](https://hub.docker.com/)


## Contact:
For any questions or suggestions, feel free to reach out:

- GitHub: [github.com/kevalhingu](https://github.com/kevalhingu)
- LinkedIn: [linkedin.com/in/kevalhingu](https://www.linkedin.com/in/keval-hingu-499211219/)

Happy coding!
