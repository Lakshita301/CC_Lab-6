# Automated CI/CD Deployment with Jenkins & NGINX Load Balancing

This repository contains an automated deployment pipeline built using **Jenkins** and **NGINX** to build, test, and distribute traffic across containerized backend services running inside Docker[cite: 3].

## 📌 Architecture
Client Request -> NGINX Load Balancer (Port 80) -> backend1 / backend2 (Port 8080)
---

## 📁 Repository Structure
CC_LAB-6/
├── Dockerfile.jenkins  # Custom Jenkins image definition with Docker CLI support
├── Jenkinsfile        # Declarative CI/CD pipeline definition
├── backend/           # Web app source code & Dockerfile
└── nginx/             # Proxy configuration files
└── default.conf   # Upstream load balancing rules
---

## 🚀 Setup Instructions

### Task 1: Run Custom Jenkins Container
Build and start the custom Jenkins instance with mounted Docker daemon socket[cite: 3]:
```bash
docker build -t jenkins-docker -f Dockerfile.jenkins .

docker run -d -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  --user root \
  --name jenkins jenkins-docker
```[cite: 3]

Retrieve initial unlock password[cite: 3]:
```bash
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```[cite: 3]

Navigate to `http://localhost:8080` to complete setup[cite: 3].

---

### Task 2: Parameterized Pipeline
1. Create a new Pipeline job in Jenkins named `LAB6-PIPELINE-NGINX`[cite: 3].
2. Set **Definition** to `Pipeline script from SCM` using Git and point to `CC_LAB-6/Jenkinsfile`[cite: 3].
3. Run **Build with Parameters** (`Backend_Count = 2`) to dynamically deploy 2 backend containers[cite: 3].

---

### Task 3: Load Balancing Strategies

Modify `nginx/default.conf` to test different routing strategies[cite: 3]:

#### 1. Round-Robin (Default)
Distributes incoming requests sequentially[cite: 3]:
```nginx
upstream backend_servers {
    server backend1:8080;
    server backend2:8080;
}
```[cite: 3]

#### 2. Least Connections (`least_conn`)
Forwards requests to the server with fewest active connections[cite: 3]:
```nginx
upstream backend_servers {
    least_conn;
    server backend1:8080;
    server backend2:8080;
}
```[cite: 3]

#### 3. IP Hash (`ip_hash`)
Binds client IP to a specific backend for session persistence[cite: 3]:
```nginx
upstream backend_servers {
    ip_hash;
    server backend1:8080;
    server backend2:8080;
}
```[cite: 3]

Commit changes to GitHub and trigger **Build Now** in Jenkins to apply configurations automatically[cite: 3].

---

## 🔧 Troubleshooting

- **Docker Daemon Permission Errors:**
  Ensure Jenkins container is run with `--user root` to access `/var/run/docker.sock`[cite: 3].
- **NGINX 502 Bad Gateway:**
  Verify backend containers are running (`docker ps`) and ensure a `sleep` delay exists in the `Jenkinsfile` before reloading NGINX configs[cite: 3].
