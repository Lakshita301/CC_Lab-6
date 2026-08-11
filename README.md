# Automated Container Deployment & Load Balancing

## Overview

A containerized CI/CD deployment setup using **Jenkins, Docker, GitHub, and NGINX**.

Jenkins automates the build and deployment of backend services, while NGINX acts as a reverse proxy and load balancer to distribute incoming requests across multiple backend containers.

## Architecture

```text
GitHub
   │
   ▼
Jenkins
   │
   ▼
Docker
   │
   ├──► Backend 1
   │
   └──► Backend 2
          │
          ▼
       NGINX
          │
          ▼
        Client
Technologies
Jenkins – CI/CD automation
Docker – Containerization
NGINX – Reverse proxy & load balancing
GitHub – Source control
Jenkins Pipeline – Automated deployment
Features
Automated Docker image builds through Jenkins
Parameterized deployment of backend containers
CI/CD pipeline defined using a Jenkinsfile
NGINX reverse proxy configuration
Multiple backend container deployment
Support for multiple load-balancing strategies:
Round-Robin
Least Connections
IP Hash
Project Structure
CC_Lab-6/
├── backend/
│   └── Dockerfile
├── nginx/
│   └── default.conf
├── Dockerfile.jenkins
├── Jenkinsfile
└── README.md
Setup
1. Build Jenkins Image
docker build -t jenkins-docker -f Dockerfile.jenkins .
2. Run Jenkins
docker run -d \
  -p 8080:8080 \
  -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --user root \
  --name jenkins \
  jenkins-docker

Jenkins runs at:

http://localhost:8080

The deployed application is accessible through NGINX at:

http://localhost
Load Balancing

NGINX distributes requests between the backend containers using configurable strategies.

Round-Robin

Requests are distributed sequentially between the available backend instances.

Least Connections

Requests are directed toward the backend with fewer active connections.

IP Hash

Requests from the same client are consistently routed to the same backend instance.

CI/CD Workflow
Code Push
    │
    ▼
GitHub Repository
    │
    ▼
Jenkins Pipeline
    │
    ├── Build Backend Image
    │
    ├── Deploy Backend Containers
    │
    └── Deploy NGINX
            │
            ▼
       Load Balanced
        Application
Outcome

This project demonstrates a basic containerized CI/CD workflow where source code can be built and deployed automatically through Jenkins, with NGINX providing traffic distribution across multiple backend instances.
