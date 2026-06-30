# GitLab DevSecOps Pipeline with GitOps, Blue-Green Deployment & Cosign Image Signing on AWS EKS

## Overview

This project demonstrates a complete **end-to-end DevSecOps implementation** using **GitLab CI/CD**, **Amazon EKS**, **GitOps**, **Argo CD**, **Argo Rollouts**, and **Cosign** to securely build, scan, sign, verify, and deploy containerized applications.

The pipeline follows **Shift-Left Security** principles by integrating multiple security gates before deployment, ensuring that only scanned and cryptographically signed container images are promoted to Kubernetes.

---

# Features

* GitLab CI/CD Pipeline
* Docker Image Build
* Amazon ECR Integration
* Kubernetes Deployment on Amazon EKS
* GitOps using Argo CD
* Blue-Green Deployment using Argo Rollouts
* Secret Detection with Gitleaks
* Static Application Security Testing (SAST) using SonarQube
* Infrastructure as Code (IaC) Security using Checkov
* Container Vulnerability Scanning using Trivy
* Container Image Signing using Cosign
* Container Image Signature Verification
* Infrastructure Provisioning using Terraform

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
     ┌──────────────┬──────────────┬──────────────┐
     │              │              │              │
     ▼              ▼              ▼              ▼
 Gitleaks      SonarQube       Checkov      Docker Build
 Secrets          SAST          IaC Scan
   Scan
                           │
                           ▼
                 Push Image to Amazon ECR
                           │
                           ▼
                  Trivy Image Scan
                           │
                           ▼
                  Cosign Image Signing
                           │
                           ▼
             Cosign Signature Verification
                           │
                           ▼
             Update GitOps Deployment Manifest
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
                     Amazon EKS
```

---

# Technology Stack

| Technology    | Purpose                                      |
| ------------- | -------------------------------------------- |
| GitLab CI/CD  | Continuous Integration & Continuous Delivery |
| Docker        | Containerization                             |
| Amazon ECR    | Container Registry                           |
| Kubernetes    | Container Orchestration                      |
| Amazon EKS    | Managed Kubernetes                           |
| Terraform     | Infrastructure Provisioning                  |
| SonarQube     | Static Application Security Testing (SAST)   |
| Gitleaks      | Secret Detection                             |
| Checkov       | Infrastructure as Code Security              |
| Trivy         | Container Vulnerability Scanning             |
| Cosign        | Container Image Signing & Verification       |
| Argo CD       | GitOps Continuous Delivery                   |
| Argo Rollouts | Blue-Green Deployment                        |

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

# DevSecOps Pipeline

## Stage 1 – Docker Build

* Builds Docker image
* Tags image using Git commit SHA
* Pushes image to Amazon ECR

Example:

```yaml
docker_build:
  stage: build
```

---

## Stage 2 – Secret Detection

Tool:

**Gitleaks**

Scans the repository for:

* Hardcoded Passwords
* AWS Access Keys
* API Tokens
* Secrets
* Certificates
* Private Keys

Example:

```yaml
git_leaks:
  stage: security
```

---

## Stage 3 – Static Application Security Testing (SAST)

Tool:

**SonarQube**

Performs:

* Code Quality Analysis
* Security Hotspots
* Vulnerability Detection
* Code Smells
* Technical Debt Analysis

Example:

```yaml
sonarqube_scan:
  stage: security
```

---

## Stage 4 – Infrastructure as Code Security

Tool:

**Checkov**

Scans:

* Terraform
* Kubernetes Manifests

Detects:

* Security Misconfigurations
* IAM Issues
* Public Exposure
* Compliance Violations
* Encryption Issues

Example:

```yaml
checkov_scan:
  stage: security
```

---

## Stage 5 – Container Security

Tool:

**Trivy**

Scans Docker images for:

* Critical CVEs
* High Severity Vulnerabilities
* OS Package Issues
* Dependency Vulnerabilities

Example:

```yaml
trivy-scan:
  stage: security
```

---

# Software Supply Chain Security

This project implements **container image signing** using **Cosign**.

Every container image pushed to Amazon ECR is digitally signed before deployment.

The pipeline then verifies the signature before updating the GitOps deployment manifests.

This ensures that only trusted and verified container images are deployed.

Pipeline Flow

```text
Docker Build
      │
      ▼
Push to Amazon ECR
      │
      ▼
Cosign Sign
      │
      ▼
Cosign Verify
      │
      ▼
Update GitOps Manifest
```

Benefits

* Image Integrity
* Image Authenticity
* Supply Chain Security
* Tamper Detection
* Trusted Deployments

---

# GitOps Deployment

Argo CD continuously watches the Kubernetes manifests stored in Git.

Whenever the deployment manifest changes, Argo CD automatically synchronizes the cluster with the desired state.

Workflow

```text
Developer Updates Manifest
           │
           ▼
      Git Repository
           │
           ▼
        Argo CD
           │
           ▼
      Auto Synchronization
           │
           ▼
      Amazon EKS Cluster
```

Key Features

* Declarative Deployments
* Git as Single Source of Truth
* Drift Detection
* Self-Healing
* Automatic Synchronization

---

# Progressive Delivery

The application is deployed using **Blue-Green Deployment** with Argo Rollouts.

Deployment Flow

```text
Version 1 (Blue)
       │
       ▼
Version 2 (Green)
       │
       ▼
Preview Validation
       │
       ▼
Manual Promotion
       │
       ▼
Traffic Switch
       │
       ▼
Production
```

Benefits

* Zero Downtime
* Preview Environment
* Manual Approval
* Instant Rollback
* Progressive Delivery

---

# Security Gates

| Security Gate        | Tool      | Purpose                                     |
| -------------------- | --------- | ------------------------------------------- |
| Secret Detection     | Gitleaks  | Detect hardcoded secrets                    |
| Static Code Analysis | SonarQube | Detect code vulnerabilities                 |
| IaC Security         | Checkov   | Scan Terraform & Kubernetes manifests       |
| Container Security   | Trivy     | Scan container images                       |
| Image Signing        | Cosign    | Digitally sign container images             |
| Image Verification   | Cosign    | Verify image authenticity before deployment |

---

# Kubernetes Deployment

Deployment Components

* Deployment
* Rollout
* Service
* Active Service
* Preview Service
* Ingress
* Namespace

Features

* High Availability
* Scalability
* Cloud Native
* GitOps Deployment
* Progressive Delivery

---

# AWS Integration

This project integrates with AWS services including:

* Amazon EKS
* Amazon ECR
* AWS IAM
* Elastic Load Balancer
* Auto Scaling

---

# Setup

## Clone Repository

```bash
git clone https://github.com/<your-github-username>/<repository-name>.git
```

---

## Provision Infrastructure

```bash
terraform init

terraform plan

terraform apply
```

---

## Verify EKS Cluster

```bash
kubectl get nodes
```

---

## Deploy Argo CD Application

```bash
kubectl apply -f application.yaml
```

---

## Verify Argo CD

```bash
kubectl get applications -n argocd
```

---

## Verify Pods

```bash
kubectl get pods -A
```

---

# Argo Rollouts Commands

```bash
kubectl argo rollouts get rollout nginx-demo -n default

kubectl argo rollouts dashboard

kubectl argo rollouts promote nginx-demo -n default

kubectl argo rollouts undo nginx-demo -n default
```

---

# Sample Security Findings

## Gitleaks

* AWS Credentials
* API Tokens
* Passwords
* SSH Keys

---

## SonarQube

* Security Hotspots
* Code Smells
* Bugs
* Vulnerabilities

---

## Checkov

* Public Security Groups
* IAM Misconfigurations
* Missing Encryption
* Kubernetes Security Issues

---

## Trivy

* Critical CVEs
* High Severity Vulnerabilities
* Outdated Packages
* Dependency Risks

---

## Cosign

* Image Signature Verification
* Trusted Image Validation
* Tamper Detection

---

# Learning Outcomes

This project demonstrates hands-on experience with:

* DevSecOps
* GitLab CI/CD
* GitOps
* Docker
* Kubernetes
* Amazon EKS
* Amazon ECR
* Terraform
* SonarQube
* Gitleaks
* Checkov
* Trivy
* Cosign
* Argo CD
* Argo Rollouts
* Blue-Green Deployment
* Software Supply Chain Security
* Shift-Left Security
* Infrastructure as Code
* Cloud Native Deployments

---

# Future Enhancements

* Helm-based Deployments
* Multi-Environment GitOps
* Multi-Cluster Deployments
* SBOM Generation
* SLSA Provenance
* Kyverno Image Signature Validation
* OPA Gatekeeper Policies
* Falco Runtime Security
* Prometheus & Grafana Monitoring
* OpenTelemetry Observability

---

# Author

**Shrini**

**DevOps | DevSecOps | Cloud Platform Engineer**

GitHub:
https://github.com/shrini-devsecops

LinkedIn:
https://linkedin.com/in/shrinivasa-a-l-devops
