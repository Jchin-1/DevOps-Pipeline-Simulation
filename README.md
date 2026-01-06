# SecureFlow-Pipeline

## Overview

SecureFlow-Pipeline is a demonstration of a **DevSecOps** approach in a CI/CD pipeline. It features a specific focus on "Shift-Left" security, ensuring that vulnerabilities are detected and blocked early in the development lifecycle before they can reach production.

## Pipeline Architecture

The following diagram illustrates the SecureFlow pipeline, where every commit triggers a build and a security scan. High-severity vulnerabilities automatically block the deployment.

```mermaid
graph LR
    A[Commit / Push] --> B[Build Docker Image]
    B --> C[Security Scan (Trivy)]
    C --> D{Vulnerabilities Found?}
    D -- Yes (Critical/High) --> E[Fail Build / Block Deploy]
    D -- No --> F[Push to Registry]
    F --> G[Deploy]
```

## Security Architecture

### Why Shift-Left?
In a traditional banking environment, security checks often happen at the end of the release cycle. This "Shift-Left" approach moves security testing into the earliest stages of development.

### Vulnerability Blocking
We utilize **Trivy** to scan the container image for Common Vulnerabilities and Exposures (CVEs).
- **Threshold**: The pipeline is configured to fail (`exit-code: 1`) if any `CRITICAL` or `HIGH` severity vulnerabilities are detected.
- **Compliance**: This ensures that no code with known exploitable high-risk vulnerabilities can ever be pushed to the artifact registry, adhering to strict financial compliance standards (e.g., PCI-DSS, SOC2).

## Getting Started

### Prerequisites
- Docker
- GitHub Repository with Actions enabled
- Docker Hub Account

### Secrets Configuration
To run this pipeline, configure the following **GitHub Secrets**:
- `DOCKER_USERNAME`: Your Docker Hub username.
- `DOCKER_PASSWORD`: Your Docker Hub access token.

## Tech Stack
- **Application**: Python Flask
- **Containerization**: Docker
- **CI/CD**: GitHub Actions
- **Security**: Trivy (Aqua Security)
