# GitLab DevSecOps Pipeline with GitOps & Blue-Green Deployment on AWS EKS

A complete end-to-end DevSecOps implementation using GitLab CI/CD integrated with security scanning tools, Kubernetes, Argo CD, Argo Rollouts, and AWS EKS.

---

# Project Overview

This project demonstrates a practical DevSecOps workflow where application code, infrastructure, and Kubernetes deployment manifests are continuously validated for security and compliance before deployment.

The solution incorporates:

* GitLab CI/CD
* Docker
* Kubernetes
* Amazon EKS
* SonarQube
* Gitleaks
* Trivy
* Checkov
* Argo CD
* Argo Rollouts
* Terraform
* GitOps Deployment Model
* Blue-Green Deployment Strategy

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
 ┌──────┼─────────────────────────────┐
 │      │             │               │
 ▼      ▼             ▼               ▼
Gitleaks SonarQube  Checkov       Docker Build
Secrets   SAST      IaC Scan
 Scan
        │
        ▼
 Kubernetes Manifests
        │
        ▼
      Argo CD
        │
        ▼
   Argo Rollouts
        │
        ▼
 Blue-Green Deployment
        │
        ▼
      AWS EKS
```

---

# Tools Used

| Tool          | Purpose                                       |
| ------------- | --------------------------------------------- |
| GitLab CI/CD  | Continuous Integration & Delivery             |
| Docker        | Containerization                              |
| Kubernetes    | Container Orchestration                       |
| AWS EKS       | Managed Kubernetes Service                    |
| SonarQube     | Static Application Security Testing (SAST)    |
| Gitleaks      | Secret Detection                              |
| Checkov       | Infrastructure as Code Security Scanning      |
| Trivy         | Container Vulnerability Scanning              |
| Argo CD       | GitOps Continuous Delivery                    |
| Argo Rollouts | Progressive Delivery & Blue-Green Deployments |
| Terraform     | Infrastructure Provisioning                   |

---

# Repository Structure

```text
.
├── app.js
├── Dockerfile
├── main.tf
├── sonar-project.properties
├── .gitlab-ci.yml
├── application.yaml
│
├── k8s-manifests
│   ├── deployment.yaml
│   ├── rollout.yaml
│   ├── service.yaml
│   ├── service-active.yaml
│   ├── service-preview.yaml
│   └── ingress.yaml
│
├── secrets.env
├── aws-secret.txt
└── README.md
```

---

# DevSecOps Pipeline Stages

## 1. Secret Scanning

Gitleaks scans the repository for:

* Hardcoded passwords
* AWS Access Keys
* API Tokens
* Credentials
* Sensitive information

Example:

```yaml
git_leaks:
  stage: security
```

---

## 2. Static Application Security Testing (SAST)

SonarQube performs:

* Code quality analysis
* Security hotspot detection
* Vulnerability identification
* Technical debt analysis

Example:

```yaml
sonarqube_scan:
  stage: security
```

---

## 3. Infrastructure as Code Security

Checkov scans Terraform and Kubernetes manifests for:

* Misconfigurations
* Compliance violations
* Public exposure risks
* Insecure IAM policies

Example:

```yaml
checkov_scan:
  stage: security
```

---

## 4. Container Security Scanning

Trivy scans:

* Container images
* Operating system packages
* Dependencies
* Vulnerabilities

Example:

```yaml
trivy-scan:
  stage: security
```

---

## 5. Docker Image Build

Application images are built using Docker.

Example:

```yaml
docker-build:
  stage: build
```

---

# GitOps Deployment with Argo CD

This project demonstrates GitOps-based deployment using Argo CD.

Argo CD continuously monitors Kubernetes manifests stored in Git and automatically synchronizes the desired state to the Kubernetes cluster.

## Workflow

```text
Developer Updates Kubernetes Manifest
                │
                ▼
         GitLab Repository
                │
                ▼
      Argo CD Detects Change
                │
                ▼
      Application OutOfSync
                │
                ▼
           Auto Sync
                │
                ▼
         AWS EKS Deployment
```

## Key GitOps Features

* Declarative Deployments
* Automatic Synchronization
* Self-Healing
* Drift Detection
* Git as Single Source of Truth

## Example Argo CD Application

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx-demo
  namespace: argocd

spec:
  project: default

  source:
    repoURL: <gitlab-repository>
    targetRevision: argocd-deployment
    path: k8s-manifests

  destination:
    server: https://kubernetes.default.svc
    namespace: default

  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

---

# Progressive Delivery using Argo Rollouts

This project implements Blue-Green deployment using Argo Rollouts integrated with Argo CD on Amazon EKS.

## Deployment Workflow

```text
Developer Updates Image Version
              │
              ▼
      GitLab Repository
              │
              ▼
         Argo CD Sync
              │
              ▼
 Argo Rollouts Creates
   Preview Environment
              │
              ▼
      Validation Testing
              │
              ▼
      Manual Promotion
              │
              ▼
      Traffic Switch
              │
              ▼
      Production Release
```

## Blue-Green Deployment Features

* Zero Downtime Deployments
* Preview Environment Validation
* Manual Promotion Approval
* Instant Rollback Capability
* Traffic Switching
* Kubernetes Native Progressive Delivery

## Example Rollout Configuration

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout

metadata:
  name: nginx-demo

spec:
  strategy:
    blueGreen:
      activeService: nginx-active
      previewService: nginx-preview
      autoPromotionEnabled: false
```

---

# Kubernetes Deployment

The application is deployed to Amazon EKS using Kubernetes manifests.

Deployment Components:

* Deployment
* Rollout
* Service
* Ingress
* Namespace
* Containerized Application

Benefits:

* High Availability
* Scalability
* Declarative Infrastructure
* Cloud-Native Deployment Model

---

# Security Controls Implemented

| Security Area               | Implementation |
| --------------------------- | -------------- |
| Secret Detection            | Gitleaks       |
| SAST                        | SonarQube      |
| IaC Security                | Checkov        |
| Container Security          | Trivy          |
| GitOps Deployment           | Argo CD        |
| Progressive Delivery        | Argo Rollouts  |
| Kubernetes Deployment       | Amazon EKS     |
| Infrastructure Provisioning | Terraform      |

---

# AWS EKS Integration

This project integrates with Amazon EKS for Kubernetes deployment.

Features:

* Managed Kubernetes Control Plane
* Secure Cluster Deployment
* Load Balancer Integration
* Auto Scaling Support
* Cloud-Native Application Hosting

---

# Key Features

* End-to-End DevSecOps Pipeline
* Shift-Left Security
* GitOps Deployment Model
* Blue-Green Deployment Strategy
* Progressive Delivery
* Infrastructure as Code
* Kubernetes Security Best Practices
* CI/CD Automation
* Automated Security Gates
* Cloud-Native Architecture

---

# Setup Instructions

## Clone Repository

```bash
git clone https://github.com/<your-github-username>/<repository-name>.git
```

## Run Terraform

```bash
terraform init
terraform plan
terraform apply
```

## Verify Kubernetes Cluster

```bash
kubectl get nodes
```

## Configure Argo CD

```bash
kubectl apply -f application.yaml
```

## Verify Application

```bash
kubectl get applications -n argocd
kubectl get pods
```

---

# Blue-Green Deployment Demonstration

Successfully implemented and validated Blue-Green deployment using Argo Rollouts and Argo CD.

## Commands Used

```bash
kubectl argo rollouts get rollout nginx-demo -n default

kubectl argo rollouts promote nginx-demo -n default

kubectl argo rollouts undo nginx-demo -n default

kubectl argo rollouts dashboard
```

## Deployment Lifecycle

```text
Version 1 (Blue)
       │
       ▼
Version 2 (Green Preview)
       │
       ▼
Manual Promotion
       │
       ▼
Traffic Switch
       │
       ▼
Version 2 Production
```

---

# Sample Security Findings

## Gitleaks

* Hardcoded Secrets
* API Tokens
* AWS Credentials
* Password Exposure

## SonarQube

* Code Smells
* Security Hotspots
* Maintainability Issues
* Vulnerability Detection

## Checkov

* Terraform Misconfigurations
* Security Group Violations
* IAM Policy Issues
* Kubernetes Security Checks

## Trivy

* Container Vulnerabilities
* Dependency Risks
* Critical CVEs
* High Severity Findings

---

# Learning Outcomes

This project demonstrates practical experience with:

* DevSecOps
* GitLab CI/CD
* Kubernetes
* AWS EKS
* GitOps
* Argo CD
* Argo Rollouts
* Blue-Green Deployments
* Progressive Delivery
* Terraform
* Container Security
* Infrastructure Security
* Security Automation
* Cloud Native Tooling

---

# Future Enhancements

* ECR Image Push
* Automated Image Tag Updates
* Helm-Based Deployments
* Multi-Environment GitOps
* Prometheus & Grafana Monitoring
* Kyverno Policies

---

# Author

**Shrini**

DevOps / DevSecOps Engineer

GitHub: https://github.com/shrini-devsecops

LinkedIn: https://linkedin.com/in/shrinivasa-a-l-devops
