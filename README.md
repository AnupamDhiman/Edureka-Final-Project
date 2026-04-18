# 🚀 ABC Technologies — End-to-End DevOps CI/CD Pipeline

[![Java](https://img.shields.io/badge/Java-8-orange?logo=java)](https://www.java.com/)
[![Maven](https://img.shields.io/badge/Maven-3.x-C71A36?logo=apachemaven)](https://maven.apache.org/)
[![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestration-326CE5?logo=kubernetes)](https://kubernetes.io/)
[![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-D24939?logo=jenkins)](https://www.jenkins.io/)
[![Ansible](https://img.shields.io/badge/Ansible-Automation-EE0000?logo=ansible)](https://www.ansible.com/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Monitoring-E6522C?logo=prometheus)](https://prometheus.io/)
[![Terraform](https://img.shields.io/badge/Terraform-IaC-7B42BC?logo=terraform)](https://www.terraform.io/)

> A complete DevOps pipeline demonstrating CI/CD, containerization, orchestration, configuration management, and monitoring for a Java web application — built as part of the **Purdue University PG Program in DevOps**.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Architecture](#-architecture)
- [Tech Stack](#-tech-stack)
- [Project Structure](#-project-structure)
- [Prerequisites](#-prerequisites)
- [Getting Started](#-getting-started)
  - [1. Build the Application](#1-build-the-application)
  - [2. Containerize with Docker](#2-containerize-with-docker)
  - [3. Deploy to Kubernetes](#3-deploy-to-kubernetes)
  - [4. Automate with Ansible](#4-automate-with-ansible)
  - [5. Set Up Monitoring](#5-set-up-monitoring)
- [CI/CD Pipeline](#-cicd-pipeline)
- [Monitoring](#-monitoring)
- [Key DevOps Concepts Demonstrated](#-key-devops-concepts-demonstrated)
- [Future Enhancements](#-future-enhancements)
- [Author](#-author)

---

## 🎯 Project Overview

**ABC Technologies** is a retail web application (Java/Maven WAR) used as a vehicle to demonstrate a production-grade DevOps pipeline. The project covers the full software delivery lifecycle:

1. **Source Code Management** — Git & GitHub for version control
2. **Build Automation** — Maven for compiling, testing, and packaging
3. **Containerization** — Docker for creating portable application images
4. **Container Orchestration** — Kubernetes for deployment, scaling, and management
5. **Configuration Management** — Ansible for automated server provisioning and app deployment
6. **CI/CD Pipeline** — Jenkins for end-to-end build and deployment automation
7. **Monitoring** — Prometheus & Grafana for infrastructure and application metrics
8. **Infrastructure as Code** — Terraform for cloud resource provisioning

---

## 🏗 Architecture

```
┌─────────────┐     ┌──────────┐     ┌──────────┐     ┌────────────────┐
│  Developer   │────▶│  GitHub   │────▶│ Jenkins  │────▶│  Maven Build   │
│  (Git Push)  │     │  (SCM)   │     │ (CI/CD)  │     │  (Compile/Test)│
└─────────────┘     └──────────┘     └──────────┘     └───────┬────────┘
                                                               │
                                                               ▼
┌─────────────┐     ┌──────────────┐     ┌──────────┐     ┌──────────┐
│ Prometheus  │◀────│  Kubernetes  │◀────│ Ansible  │◀────│  Docker  │
│  & Grafana  │     │  (K8s Cluster)│    │(Automate)│     │ (Build & │
│ (Monitoring)│     │  2 Replicas  │     └──────────┘     │   Push)  │
└─────────────┘     └──────────────┘                      └──────────┘
```

---

## 🛠 Tech Stack

| Category | Technology | Purpose |
|----------|-----------|---------|
| **Language** | Java 8 | Application development |
| **Build Tool** | Maven 3.x | Dependency management, build, and packaging |
| **Containerization** | Docker | Application containerization |
| **Orchestration** | Kubernetes | Container orchestration and scaling |
| **CI/CD** | Jenkins | Continuous Integration & Delivery pipeline |
| **Config Management** | Ansible | Automated server setup and deployment |
| **Monitoring** | Prometheus + Grafana | Metrics collection and visualization |
| **IaC** | Terraform | Cloud infrastructure provisioning |
| **Version Control** | Git & GitHub | Source code management |
| **Code Quality** | JaCoCo + SonarQube | Code coverage and static analysis |
| **App Server** | Apache Tomcat 9 | WAR deployment and serving |

---

## 📁 Project Structure

```
Edureka-Final-Project/
│
├── src/                          # Java source code
│   ├── main/
│   │   ├── java/                 # Application Java classes
│   │   └── webapp/               # Web application resources (JSP, HTML, CSS)
│   └── test/                     # Unit test classes
│
├── Dockerfile                    # Docker image definition (Ubuntu + JDK8 + Tomcat 9)
├── Deployment.yml                # Kubernetes Deployment manifest (2 replicas)
├── Service.yml                   # Kubernetes Service manifest (NodePort)
├── play1.yml - Ansible playbook  # Ansible playbook for automated deployment
├── prometheus.yml                # Prometheus monitoring configuration
├── pom.xml                       # Maven project configuration with JaCoCo
├── .classpath                    # Eclipse IDE configuration
├── .project                      # Eclipse project metadata
└── README.md                     # Project documentation (this file)
```

---

## ✅ Prerequisites

Ensure the following tools are installed on your system:

| Tool | Version | Installation Guide |
|------|---------|-------------------|
| Java JDK | 8+ | [Download](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html) |
| Maven | 3.x | [Install Guide](https://maven.apache.org/install.html) |
| Docker | 20.x+ | [Get Docker](https://docs.docker.com/get-docker/) |
| Kubernetes (kubectl) | 1.24+ | [Install kubectl](https://kubernetes.io/docs/tasks/tools/) |
| Minikube (for local) | Latest | [Install Minikube](https://minikube.sigs.k8s.io/docs/start/) |
| Ansible | 2.9+ | [Install Ansible](https://docs.ansible.com/ansible/latest/installation_guide/) |
| Jenkins | 2.x | [Install Jenkins](https://www.jenkins.io/doc/book/installing/) |
| Prometheus | 2.27+ | [Download](https://prometheus.io/download/) |

---

## 🚀 Getting Started

### 1. Build the Application

```bash
# Clone the repository
git clone https://github.com/AnupamDhiman/Edureka-Final-Project.git
cd Edureka-Final-Project

# Build with Maven (compiles, runs tests, generates WAR)
mvn clean package

# Run tests with code coverage (JaCoCo)
mvn test
```

The WAR file will be generated at `target/ABCtechnologies-1.0.war`.

---

### 2. Containerize with Docker

```bash
# Build the Docker image
docker build -t anupamdhiman/abctechnologies:latest .

# Run the container locally
docker run -d -p 8080:8080 --name abc-app anupamdhiman/abctechnologies:latest

# Verify the application is running
curl http://localhost:8080

# Push to Docker Hub
docker login
docker push anupamdhiman/abctechnologies:latest
```

**Dockerfile Overview:**
- Base image: Ubuntu 18.04
- Installs OpenJDK 8 and Apache Tomcat 9
- Copies the WAR file into Tomcat's webapps directory
- Exposes ports 8080 and 8081

---

### 3. Deploy to Kubernetes

```bash
# Start Minikube (for local testing)
minikube start

# Create Deployment (2 replicas)
kubectl apply -f Deployment.yml

# Create Service (NodePort)
kubectl apply -f Service.yml

# Verify pods are running
kubectl get pods -l app=test1

# Get the service URL
minikube service svc1 --url
```

**Kubernetes Configuration:**
- **Deployment:** 2 replicas of the containerized app for high availability
- **Service:** NodePort type for external access to the application

---

### 4. Automate with Ansible

```bash
# Run the Ansible playbook on target hosts
ansible-playbook -i inventory play1.yml
```

**What the Ansible playbook does:**
1. Installs Docker on target servers
2. Pulls and runs the Docker container
3. Creates Kubernetes Deployment and Service
4. Handles Docker service restart via handlers

---

### 5. Set Up Monitoring

```bash
# Start Prometheus with the provided config
prometheus --config.file=prometheus.yml

# Access Prometheus UI
# http://localhost:9090

# Set up Grafana (via Docker)
docker run -d -p 3000:3000 grafana/grafana

# Access Grafana UI
# http://localhost:3000 (admin/admin)
# Add Prometheus as a data source → http://localhost:9090
```

**Prometheus monitors:**
- `node_exporter` on port 9100 (system metrics: CPU, memory, disk)
- Prometheus self-metrics on port 9090
- Scrape interval: 5 seconds for node metrics, 15 seconds globally

---

## 🔄 CI/CD Pipeline

The Jenkins CI/CD pipeline automates the entire workflow:

```
┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐
│  Git     │──▶│  Maven   │──▶│  Docker  │──▶│  Push to │──▶│  Deploy  │
│  Clone   │   │  Build   │   │  Build   │   │  Docker  │   │  to K8s  │
│          │   │  & Test  │   │  Image   │   │   Hub    │   │          │
└──────────┘   └──────────┘   └──────────┘   └──────────┘   └──────────┘
```

**Pipeline Stages:**
1. **Checkout** — Pull latest code from GitHub
2. **Build** — Compile Java code and run unit tests with Maven
3. **Code Quality** — Generate JaCoCo coverage report (SonarQube integration ready)
4. **Docker Build** — Build Docker image with the WAR artifact
5. **Docker Push** — Push image to Docker Hub registry
6. **Deploy** — Deploy to Kubernetes cluster via Ansible automation

---

## 📊 Monitoring

| Component | Tool | Endpoint |
|-----------|------|----------|
| Infrastructure Metrics | Prometheus + Node Exporter | `http://localhost:9090` |
| Dashboards & Alerts | Grafana | `http://localhost:3000` |
| System Metrics | Node Exporter | `http://localhost:9100` |

**Metrics Collected:**
- CPU utilization, Memory usage, Disk I/O
- Network traffic and bandwidth
- Container and pod health (when running on K8s)

---

## 🎓 Key DevOps Concepts Demonstrated

| Concept | Implementation |
|---------|---------------|
| **Version Control** | Git branching, commits, and GitHub repository management |
| **CI/CD** | Jenkins pipeline automating build → test → deploy |
| **Containerization** | Dockerfile creating portable, reproducible application images |
| **Orchestration** | Kubernetes Deployments with replicas and Services for networking |
| **Configuration Management** | Ansible playbooks for automated server provisioning |
| **Infrastructure as Code** | Terraform for cloud resource management |
| **Monitoring & Observability** | Prometheus metrics collection + Grafana dashboards |
| **Code Quality** | JaCoCo for test coverage + SonarQube integration |

---

## 🔮 Future Enhancements

- [ ] Add Jenkinsfile (Pipeline-as-Code) to the repository
- [ ] Implement Helm charts for Kubernetes deployments
- [ ] Add Terraform scripts for AWS infrastructure provisioning
- [ ] Set up Grafana alerting for critical metrics
- [ ] Implement blue-green / canary deployment strategies
- [ ] Add `.gitignore` to exclude IDE and build artifacts
- [ ] Implement multi-stage Docker builds for smaller images
- [ ] Add health check endpoints (liveness & readiness probes)
- [ ] Set up GitHub Actions as an alternative CI/CD pipeline

---

## 👤 Author

**Anupam Dhiman**
- 🔗 [LinkedIn](https://www.linkedin.com/in/anupam-dhiman-61276b38/)
- 🐙 [GitHub](https://github.com/AnupamDhiman)
- 📧 aanupammdhiman@gmail.com

---

## 📄 License

This project is part of the **Purdue University Post Graduate Program in DevOps** (via Edureka) and is intended for educational and portfolio purposes.

---

⭐ **If you found this project helpful, please give it a star!**
