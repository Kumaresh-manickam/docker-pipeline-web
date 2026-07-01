# 🚀 Automated CI/CD Web Application Deployment Pipeline

A fully automated, production-grade DevOps pipeline that containerizes a web application using **Docker** and continuously deploys it to an **AWS EC2** instance via a system-daemonized **GitHub Actions Self-Hosted Runner**.

---

## 🛠️ Infrastructure & Tech Stack
*   **Cloud Provider:** Amazon Web Services (AWS EC2 - Ubuntu 26.04 LTS)
*   **Container Runtime:** Docker Engine
*   **Orchestration & CI/CD:** GitHub Actions (Self-Hosted Background Runner)
*   **Application Layer:** Isolated Nginx/HTML Container

---

## 🏗️ Architecture Workflow
1.  **Code Change:** Developer commits and pushes changes to the `main` branch.
2.  **Pipeline Event Hook:** GitHub Actions detects the push event.
3.  **Local Server Trigger:** The background system runner (`systemd` daemon) on AWS intercepts the build task.
4.  **Container Compilation:** The EC2 server builds a fresh Docker image natively using the repository's `Dockerfile`.
5.  **Zero-Downtime Deployment:** The runner stops the legacy container, prunes old layers, and launches the updated container on public web port `8080`.

---

## 🚀 Getting Started

### 1. Prerequisites on AWS Host
Ensure Docker is initialized and your user profile has permission boundaries lifted:
```bash
sudo apt update -y
sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo usermod -aG docker ubuntu

Registering the Automated Background Service
To ensure the pipeline stays highly available and survives server restarts, the runner is configured as a background system service:
```bash
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status

Workflow Configuration
The deployment pipeline targets this internal self-hosted network cluster via the configuration file .github/workflows/deploy.yml:

YAML
jobs:
  deploy:
    runs-on: self-hosted
    steps:
      - name: Checkout Code Base
        uses: actions/checkout@v4
