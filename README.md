# DevOps Learning Project 2 🚀

![Project Banner](https://img.shields.io/badge/DevOps-Project%202-blue?style=for-the-badge&logo=jenkins)
![Next.js](https://img.shields.io/badge/Next.js-14-black?style=flat-square&logo=next.js)
![Python](https://img.shields.io/badge/Python-Flask-yellow?style=flat-square&logo=python)
![Docker](https://img.shields.io/badge/Docker-Compose-blue?style=flat-square&logo=docker)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red?style=flat-square&logo=jenkins)
![Ansible](https://img.shields.io/badge/Ansible-Infrastructure-black?style=flat-square&logo=ansible)

## 📖 Project Overview

This project is a comprehensive **DevOps Learning Resource** designed to demonstrate advanced CI/CD concepts, infrastructure automation, and containerization. It builds upon the foundations of Project 1, introducing complex pipeline chaining, Ansible automation, and dynamic environment management.

**🔗 Live Demo:** [https://devops2.himanmanduja.fun](https://devops2.himanmanduja.fun)
**📂 GitHub Repo:** [https://github.com/HimanM/DevOps-Project-2](https://github.com/HimanM/DevOps-Project-2)

---

## 🏗️ Architecture

The project consists of a modern web application stack deployed via a robust CI/CD pipeline.

### Tech Stack
*   **Frontend**: Next.js 14 (React), Tailwind CSS, Shadcn UI, Framer Motion.
*   **Backend**: Python (Flask) API.
*   **Database/Infra**: Docker, Docker Compose, Nginx (Reverse Proxy).
*   **Automation**: Jenkins (CI/CD), Ansible (Configuration Management).
*   **Monitoring**: Prometheus, Grafana.

### Infrastructure Flow
1.  **Code Push**: Developer pushes code to GitHub.
2.  **CI Pipeline (`DevOps-CI`)**: Jenkins detects change -> Lints -> Tests -> Builds Docker Images -> Pushes to Docker Hub.
3.  **CD Trigger**: On success, CI triggers CD.
4.  **CD Pipeline (`DevOps-CD`)**: Jenkins SSHs into VPS -> Pulls images -> Restarts containers.
5.  **Infrastructure (`DevOps-InitDomain`)**: Ansible configures Nginx and SSL certificates for the domain.

---

## 🚀 Jenkins Pipelines

This project uses a **Chained Pipeline Architecture**.

![Pipeline Workflow](./docs/images/jenkins_pipeline_stage_view.png)


### 1. DevOps-CI (`Jenkinsfile`)
*   **Trigger**: GitHub Webhook (Push).
*   **Tasks**:
    *   Checkout Code.
    *   Build Docker Images (Frontend & Backend).
    *   Push to Docker Hub.
    *   **Trigger `DevOps-CD`** on success.

### 2. DevOps-CD (`Jenkinsfile.deploy`)
*   **Trigger**: Upstream (`DevOps-CI`).
*   **Tasks**:
    *   SSH into VPS.
    *   Create project directories (`~/devops-project-2`).
    *   Copy `docker-compose.yml` and monitoring configs.
    *   Deploy application (`docker compose up -d`).

### 3. DevOps-InitDomain (`Jenkinsfile.initdomain`)
*   **Trigger**: Manual (One-time setup).
*   **Tasks**:
    *   Install Ansible.
    *   Run Ansible Playbook.
    *   Configure Nginx Reverse Proxy (Port 80 -> 57002).
    *   Generate SSL Certificates (Let's Encrypt).

---

## 🛠️ Getting Started

### Prerequisites
*   Docker & Docker Compose
*   Node.js 18+
*   Python 3.9+

### Local Development
1.  **Clone the repo**:
    ```bash
    git clone https://github.com/HimanM/DevOps-Project-2.git
    cd DevOps-Project-2
    ```

2.  **Run with Docker Compose**:
    ```bash
    docker compose up --build
    ```
    *   **Frontend**: `http://localhost:3000`
    *   **Backend**: `http://localhost:5000`
    *   **Grafana**: `http://localhost:3001`

---

## 📦 Deployment Setup (Jenkins)

For detailed instructions on setting up Jenkins, credentials, and the VPS, please refer to [JENKINS_SETUP.md](./JENKINS_SETUP.md).

### Quick Secret Configuration
Ensure these secrets are set in Jenkins:
*   `dockerhub-username`: Username/Password for Docker Hub.
*   `vps-credentials`: Username/Password for VPS SSH access.
*   `vps-ip`: Secret text (VPS IP Address).
*   `DEVOPS2_DOMAIN`: Secret text (e.g., `devops2.himanmanduja.fun`).

---

## 📊 Monitoring

The stack includes a pre-configured monitoring suite.
*   **Prometheus**: Scrapes metrics from the Flask backend (`/metrics`).
*   **Grafana**: Visualizes metrics. Default login: `admin` / `admin`.

---

## 📝 License

This project is for educational purposes.
