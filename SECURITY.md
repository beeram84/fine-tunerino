# SECURITY.md

## Security & Compliance Overview

This document describes the security controls implemented for the MLOps
GPT-2 Fine-Tuning pipeline running on Azure Kubernetes Service (AKS).
The project follows security best practices for cloud infrastructure,
DevOps pipelines, and machine learning workloads.

Security focus areas: - Identity & Access Management (IAM) - Network
Security - Secret Management - Encryption (Data in Transit and at
Rest) - Container Security - Compliance and Data Protection

------------------------------------------------------------------------

# 1. Identity & Access Management (IAM)

Access follows the principle of least privilege.

## Azure Access Control

-   Azure Managed Identity is used for secure service authentication.
-   Access to services such as Azure Storage and Key Vault is granted
    through Azure RBAC.

## Kubernetes RBAC

Roles restrict cluster access: - DevOps Engineer -- manage deployments
and infrastructure - ML Engineer -- trigger training jobs and manage
pipelines - CI/CD Pipeline -- deploy application containers

Cluster-admin access is limited to platform administrators.

------------------------------------------------------------------------

# 2. Network Security

Network communication is restricted to only required services.

## Kubernetes Network Policies

A default deny policy is implemented.

Allowed traffic: - Ingress Controller → API service - API service →
training pipeline - Training pipeline → storage services

## Azure Network Security Groups (NSGs)

Allowed inbound traffic: - HTTPS (443) for external access via the
Ingress Controller

All other inbound traffic is denied.

## Ingress Security

An NGINX Ingress Controller manages external access.

Security features: - HTTPS only - TLS certificates enabled - HTTP
redirected to HTTPS

------------------------------------------------------------------------

# 3. Secret Management

Sensitive credentials are never stored in source code.

## Kubernetes Secrets

Sensitive configuration such as: - API tokens - Database credentials -
Storage keys

are stored in Kubernetes Secrets and injected into pods as environment
variables.

## Azure Key Vault

Secrets may be securely stored in Azure Key Vault.

Benefits: - Secure credential storage - Secret rotation support - Access
control via Azure RBAC

## Git Security

To prevent accidental exposure: - .env files are excluded via
.gitignore - Secrets are never committed to Git repositories - Secret
scanning tools are recommended

------------------------------------------------------------------------

# 4. Encryption

## Encryption in Transit

All external communication is protected using TLS/HTTPS.

Security measures: - TLS certificates configured on the Ingress
Controller - HTTPS enforced for all public endpoints

## Encryption at Rest

Azure provides encryption for: - Azure Managed Disks - Azure Blob
Storage - AKS persistent volumes

Model artifacts and datasets are encrypted automatically by Azure
infrastructure.

------------------------------------------------------------------------

# 5. Container Security

Security practices include: - Use of minimal base images (e.g.,
python:3.11-slim) - Containers run as non-root users - Privileged
containers disabled - Resource limits defined

## Image Vulnerability Scanning

Container images are scanned using tools such as: - Trivy - Docker
Scout - GitHub Dependabot

These scans can be integrated into CI/CD pipelines before deployment.

------------------------------------------------------------------------

# 6. Logging & Monitoring

Monitoring tools: - Prometheus for metrics - Grafana for dashboards

Key monitored metrics: - CPU and memory usage - API request rates - Pod
failures

## Audit Logs

The platform enables: - Kubernetes Audit Logs - Azure Activity Logs -
Application logs

These support security monitoring and incident investigation.

------------------------------------------------------------------------

# 7. Compliance & Data Protection

This project considers data privacy regulations including GDPR when
applicable.

## Data Types

The system may process: - User uploaded documents (PDF files) - Training
datasets - Model outputs

## Data Protection Measures

Security controls include: - Restricted access to datasets - Encrypted
storage in cloud services - Sanitization of logs when possible

## Data Retention

Temporary training files are not stored permanently.

Recommended policy: - Uploaded files deleted after processing or within
30 days.

## Data Minimization

Only necessary data required for model training is processed.

------------------------------------------------------------------------

# 8. Security Best Practices for Contributors

Contributors should: - Never commit secrets or credentials - Use
environment variables for sensitive configuration - Run containers with
minimal privileges - Keep dependencies updated - Report security
vulnerabilities responsibly

------------------------------------------------------------------------

# 9. Reporting Security Issues

If a security vulnerability is discovered, please report it responsibly
to the project maintainers. Avoid public disclosure until the issue has
been reviewed and resolved.
