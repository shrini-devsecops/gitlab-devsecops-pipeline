# GitLab DevSecOps Pipeline

A complete end-to-end DevSecOps pipeline implementation using GitLab CI/CD integrated with modern security and cloud-native tools.

This project demonstrates how to build secure CI/CD pipelines with:

* GitLab CI/CD
* Docker
* Kubernetes
* Trivy
* Checkov
* SonarQube
* Argo CD
* Terraform
* AWS EKS
* OWASP ZAP

---

# Project Overview

This repository showcases a practical DevSecOps workflow where application code, infrastructure, and container images are continuously validated for security and compliance before deployment.

The pipeline includes:

* Source Code Analysis (SAST)
* Infrastructure as Code (IaC) Scanning
* Container Vulnerability Scanning
* Docker Image Build
* Kubernetes Deployment
* GitOps Deployment with Argo CD
* Dynamic Application Security Testing (DAST)

---

# Architecture

```text
Developer Commit
        │
        ▼
GitLab Repository
        │
        ▼
GitLab CI/CD Pipeline
        │
 ┌──────┼───────────────────────────────┐
 │      │               │               │
 ▼      ▼               ▼               ▼
SonarQube  Checkov   Trivy        Docker Build
  SAST      IaC       Scan             │
                                       ▼
                              Container Registry
                                       │
                                       ▼
                                  Argo CD
                                       │
                                       ▼
                                   AWS EKS
                                       │
                                       ▼
                                 OWASP ZAP
                                   DAST Scan
```

---

# Tools Used

| Tool         | Purpose                                       |
| ------------ | --------------------------------------------- |
| GitLab CI/CD | Continuous Integration & Delivery             |
| Docker       | Containerization                              |
| Kubernetes   | Container Orchestration                       |
| AWS EKS      | Managed Kubernetes Service                    |
| Trivy        | Container & Dependency Vulnerability Scanning |
| Checkov      | Infrastructure as Code Security Scanning      |
| SonarQube    | Static Code Analysis                          |
| Argo CD      | GitOps Continuous Delivery                    |
| Terraform    | Infrastructure Provisioning                   |
| OWASP ZAP    | Dynamic Application Security Testing          |

---

# Repository Structure

```text
.
├── app/
│   ├── app.js
│   ├── package.json
│   └── Dockerfile
│
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
│
├── kubernetes/
│   ├── deployment.yaml
│   ├── service.yaml
│   └── ingress.yaml
│
├── argocd/
│   └── application.yaml
│
├── sonar-project.properties
├── .gitlab-ci.yml
└── README.md
```

---

# DevSecOps Pipeline Stages

## 1. Source Code Analysis (SAST)

SonarQube is used for:

* Code quality analysis
* Security hotspot detection
* Vulnerability identification
* Technical debt analysis

Example:

```yaml
sonarqube-check:
  stage: sast
  script:
    - sonar-scanner
```

---

## 2. Infrastructure as Code Security

Checkov scans Terraform and Kubernetes manifests for:

* Misconfigurations
* Public exposure risks
* Insecure IAM policies
* Compliance violations

Example:

```yaml
checkov-scan:
  stage: security
  script:
    - checkov -d .
```

---

## 3. Container Security Scanning

Trivy scans:

* Docker images
* OS packages
* Application dependencies
* Kubernetes manifests
* Secrets

Example:

```yaml
trivy-scan:
  stage: security
  script:
    - trivy fs .
```

---

## 4. Docker Image Build

Application images are built using Docker.

Example:

```yaml
docker-build:
  stage: build
  script:
    - docker build -t devsecops-demo .
```

---

## 5. GitOps Deployment Using Argo CD

Argo CD continuously monitors Git repositories and synchronizes Kubernetes manifests automatically.

Features:

* Declarative deployments
* Automatic sync
* Self-healing
* Drift detection

---

## 6. Kubernetes Deployment

The application is deployed to AWS EKS.

Includes:

* Deployment manifests
* Services
* Ingress
* ConfigMaps
* Secrets

---

## 7. Dynamic Application Security Testing (DAST)

OWASP ZAP performs runtime application security testing.

Detects:

* XSS
* SQL Injection
* Missing security headers
* Authentication weaknesses

Example:

```yaml
zap-scan:
  stage: dast
  script:
    - zap-baseline.py -t http://app-url
```

---

# GitLab Pipeline Example

```yaml
stages:
  - build
  - sast
  - security
  - deploy

sonarqube-check:
  stage: sast

trivy-scan:
  stage: security

checkov-scan:
  stage: security

build-image:
  stage: build

deploy:
  stage: deploy
```

---

# Security Controls Implemented

| Security Area         | Implementation          |
| --------------------- | ----------------------- |
| SAST                  | SonarQube               |
| DAST                  | OWASP ZAP               |
| IaC Security          | Checkov                 |
| Container Security    | Trivy                   |
| GitOps                | Argo CD                 |
| Kubernetes Security   | RBAC & Secure Manifests |
| Supply Chain Security | Image Scanning          |

---

# AWS EKS Integration

This project integrates with AWS EKS for Kubernetes deployment.

Features:

* Managed Kubernetes cluster
* IAM Roles for Service Accounts (IRSA)
* Cluster Autoscaler
* Secure networking
* Load Balancer integration

---

# Key Features

* End-to-End DevSecOps Pipeline
* Shift-Left Security
* Infrastructure as Code
* GitOps Deployment Model
* Kubernetes Security Best Practices
* Cloud-Native Architecture
* Automated Security Gates
* CI/CD Automation

---

# Setup Instructions

## Clone Repository

```bash
git clone https://github.com/shrini-devsecops/gitlab-devsecops-pipeline.git
```

---

## Run Terraform

```bash
terraform init
terraform plan
terraform apply
```

---

## Configure Kubernetes

```bash
kubectl get nodes
```

---

## Run GitLab Pipeline

Push code to GitLab repository.

Pipeline automatically triggers:

* SAST scans
* IaC scans
* Container scans
* Docker builds
* Kubernetes deployment

---

# Sample Security Findings

## Trivy

* Vulnerable npm packages
* OS package CVEs
* Secrets detection

## Checkov

* Public Security Groups
* Open ingress rules
* Weak IAM policies

## SonarQube

* Code smells
* Security hotspots
* Maintainability issues

---

# Future Enhancements

* Helm Charts
* Kubernetes Network Policies
* Falco Runtime Security
* Admission Controllers
* Kyverno Policies
* Multi-Environment GitOps
* Prometheus Monitoring
* Grafana Dashboards
* GitLab Auto DevOps

---

# Learning Outcomes

This project demonstrates practical experience with:

* DevSecOps
* CI/CD Pipelines
* Kubernetes
* AWS Cloud
* GitOps
* Container Security
* Terraform
* Security Automation
* Cloud Native Tooling

---

# Author

Shrini DevSecOps Engineer

GitHub:
[https://github.com/shrini-devsecops](https://github.com/shrini-devsecops)

LinkedIn:
[https://linkedin.com/in/shrinivasa-a-l-devops](https://linkedin.com/in/shrinivasa-a-l-devops)
