# Kubernetes Platform Project (DevOps Showcase)

# Description
Local Kubernetes platform simulating production architecture built with Helm, featuring microservices deployment, ingress routing, and CI pipeline for containerized applications.

# Features
- Kubernetes-based microservices deployment
- Helm chart with environment-based configuration
- NGINX Ingress for HTTP routing
- Redis and PostgreSQL integration
- CI pipeline for automated Docker builds
- Health checks (liveness/readiness probes)
- Resource limits and requests
- Secrets management via Kubernetes Secrets

# Key Highlights
- Environment-based Helm deployments (dev/prod ready)
- Stateless + stateful workloads on Kubernetes
- Externalized configuration via values.yaml
- Containerized microservices architecture
- Production-like networking with Ingress controller

# How to run locally
kind create cluster --config kind-config.yaml
kubectl apply -f ingress-nginx.yaml
helm upgrade --install my-app ./helm/my-app -f values-dev.yaml

# Access
Application is exposed via Kubernetes Ingress:
http://my-app.local

# CI/CD
GitHub Actions pipeline:
- Builds Docker image
- Pushes to DockerHub
- (Deployment step disabled for local Kubernetes cluster)

In production environments, deployment would be handled via Helm or GitOps (ArgoCD).

# Tech Stack
- Kubernetes (kind)
- Helm
- Docker
- GitHub Actions
- NGINX Ingress Controller
- Node.js
- PostgreSQL (Bitnami Helm chart)
- Redis (Bitnami Helm chart)

# What I Learned
- Kubernetes deployment lifecycle
- Helm templating and environment management
- Service discovery and ingress routing
- CI pipelines for containerized applications
- Managing stateful services in Kubernetes
- Debugging ImagePullBackOff and deployment issues

# Architecture Diagram
                        ┌───────────────┐
                        │   GitHub      │
                        └──────┬────────┘
                               │
                        ┌──────▼────────┐
                        │ GitHub Actions│
                        │ Build & Push  │
                        └──────┬────────┘
                               │
                        ┌──────▼────────┐
                        │  DockerHub    │
                        └──────┬────────┘
                               │
                  ┌────────────▼────────────┐
                  │  Kubernetes (kind)      │
                  │                         │
                  │   ┌─────────────────┐   │
Browser ───────▶  │   │ Ingress (NGINX) │   │
(my-app.local)    │   └────────┬────────┘   │
                  │            │            │
                  │     ┌──────▼──────┐     │
                  │     │  Service    │     │
                  │     └──────┬──────┘     │
                  │            │            │
                  │     ┌──────▼──────┐     │
                  │     │   Pod       │     │
                  │     │ (Node App)  │     │
                  │     └──────┬──────┘     │
                  │            │            │
                  │   ┌────────▼──────┐     │
                  │   │ PostgreSQL    │     │
                  │   └───────────────┘     │
                  │   ┌───────────────┐     │
                  │   │ Redis         │     │
                  │   └───────────────┘     │
                  └─────────────────────────┘