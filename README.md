### 🚀 SkillFlow – Learning Platform
SkillFlow is a modern learning platform built using React (frontend) and Spring Boot (backend).
This repository also demonstrates a complete CI/CD pipeline integrated with Jenkins, SonarQube, DockerHub, and ArgoCD for Kubernetes deployment.
### 📌 Features
- 📚 Learning platform with React (frontend) + Spring Boot (backend)
- 🔐 SonarQube integration for static code analysis
- 🛠️ Automated build & test with Maven
- 📦 Dockerized backend application
- 🚀 Continuous Deployment with ArgoCD
- 🔔 Notifications & reports on failures

### 🏗️ CI/CD Pipeline Overview

The pipeline is automated through Jenkins and follows these stages:

1. Checkout – Pulls code from GitHub.
2. Build & Test – Runs Maven build inside a Docker container.
3. Static Code Analysis – Sends code quality report to SonarQube.
4. Docker Build & Push – Builds a backend Docker image and pushes it to DockerHub.
5. Update Deployment File – Updates Kubernetes Deployment.yml with the new image tag.
6. ArgoCD Sync – ArgoCD automatically syncs and deploys the new version.

### 🔄 CI/CD Workflow Diagram
<img src="./Screenshot (498).png" alt="CI/CD Pipeline" width="600">

### 🐳 Docker Image

The backend service is built into a Docker image and pushed to DockerHub:

👉
```bash
fasnas/ultimate-cicd
```
