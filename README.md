## Boardgame SpringBoot Application on Kubernetes with Monitoring and Security

This project provides a streamlined approach to deploy your SpringBoot boardgame application on a Kubernetes cluster hosted on AWS while ensuring comprehensive monitoring and security. The deployment process is automated using a GitHub Actions CICD pipeline.

## Overview

- Backend: SpringBoot application for your boardgame
- Containerization: Docker for building container images
- Orchestration: Kubernetes for managing container deployments and scaling
Monitoring:
- Prometheus: Collects and stores application and system metrics
- Grafana: Visualizes collected metrics for insights
- Node Exporter: Exposes system stats as Prometheus metrics
- Blackbox Exporter: Monitors external services
- Security: Trivy (optional): Scans container images for vulnerabilities
- CI/CD: GitHub Actions workflow automates build, image push, and deployment
## Features

- Automated build and deployment process through GitHub Actions
- Containerized application for portability and scalability
- Real-time monitoring of application and system health
- Customizable dashboards for visualizing key metrics
- Optional vulnerability scanning of container images with Trivy
## Prerequisites

- Java 11+
- AWS Account (or cloud provider account) with IAM user access configured for Kubernetes cluster access
- kubectl configured to interact with your Kubernetes cluster
- Git installed
- Docker installed (for local image building; not strictly required if using a container registry)