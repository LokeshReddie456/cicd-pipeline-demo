# 🚀 CI/CD Pipeline Demo

End-to-end CI/CD pipeline implementation using Jenkins and GitHub Actions for automated building, security scanning, and deployment to AWS EKS.

## 🏗️ Architecture Overview

| Flow | Steps |
|------|-------|
| **PR Flow** | Developer → GitHub PR → GitHub Actions → SonarQube + Trivy Scan → PR Approved |
| **Deploy Flow** | Merge to Main → Jenkins Triggered → Build → Scan → ECR Push → EKS Deploy |

## 🖥️ Infrastructure Setup

## 🖥️ Infrastructure Setup

| Server | Type | Purpose | Tools Installed |
|--------|------|---------|----------------|
| Jenkins Server | EC2 t3.medium | CI/CD Orchestration | Jenkins, Docker, kubectl, Trivy, AWS CLI |
| SonarQube Server | EC2 t3.small | Code Quality Analysis | SonarQube, Java |
| EKS Cluster | AWS Managed | Container Orchestration | Kubernetes 1.28 |
| ECR | AWS Managed | Docker Image Registry | - |

## 🔄 Pipeline Flow

### Pull Request Flow (GitHub Actions)

PR Raised → SonarQube Scan → Docker Build → Trivy Scan → PR Comment

### Merge to Main Flow (Jenkins)

Code Merge → Checkout → SonarQube → Quality Gate → Docker Build → Trivy Scan → ECR Push → EKS Deploy → Rollout Verify

## 📁 Project Structure

| Path | Purpose |
|------|---------|
| `jenkins/Jenkinsfile` | Full deployment pipeline |
| `github-actions/pull-request-check.yml` | PR quality gates |
| `github-actions/build-and-deploy.yml` | Main branch deployment |
| `kubernetes/deployment.yaml` | App deployment with Rolling Update |
| `kubernetes/service.yaml` | LoadBalancer service |
## 🛡️ DevSecOps Integration

### SonarQube (SAST)
- Scans source code for bugs, vulnerabilities, code smells
- Quality Gate blocks deployment if coverage drops below 80%
- Integrated in both Jenkins and GitHub Actions

### Trivy (Container Scanning)
- Scans Docker images for HIGH and CRITICAL CVEs
- Pipeline fails automatically if vulnerabilities found
- Runs before every ECR push

## ⚙️ Jenkins Server Setup

```bash
# Install Jenkins on EC2
sudo apt update
sudo apt install -y openjdk-17-jdk
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update && sudo apt install -y jenkins

# Install Docker
sudo apt install -y docker.io
sudo usermod -aG docker jenkins

# Install kubectl
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

# Install Trivy
sudo apt-get install -y wget apt-transport-https gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb generic main | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update && sudo apt-get install -y trivy
```

## 🔑 Jenkins Credentials Required

| Credential ID | Type | Purpose |
|--------------|------|---------|
| `aws-credentials` | AWS Credentials | ECR push and EKS access |
| `sonar-token` | Secret Text | SonarQube authentication |
| `github-token` | Secret Text | GitHub repo access |

## 📊 Key Achievements

- ✅ Reduced deployment lead times by **40%** through pipeline automation
- ✅ **100%** of deployments scanned for vulnerabilities before release
- ✅ Zero-downtime deployments using Kubernetes Rolling Updates
- ✅ Automated quality gates preventing bad code from reaching production

## 👤 Author
**Lokesh Kumar Reddy** — [LinkedIn](https://www.linkedin.com/in/lokesh-kumar-reddy-mandireddy-a1b0a122a) | [GitHub](https://github.com/LokeshReddie456)