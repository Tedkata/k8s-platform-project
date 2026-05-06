# Kubernetes Platform Project (DevOps Showcase)

# Description
Local Kubernetes platform simulating production architecture built with Helm and GitOps using ArgoCD, featuring microservices deployment, ingress routing, autoscaling and CI pipeline for containerized applications.

# Features
- Kubernetes-based microservices deployment
- Helm chart with environment-based configuration
- NGINX Ingress for HTTP routing
- Redis and PostgreSQL integration
- CI pipeline for automated Docker builds
- Health checks (liveness/readiness probes)
- Resource limits and requests
- Secrets management via Kubernetes Secrets
- Horizontal Pod Autoscaling (HPA) with environment-based scaling policies
- GitOps-based deployment using ArgoCD

# Key Highlights
- Environment-based Helm deployments (dev/prod ready)
- Stateless + stateful workloads on Kubernetes
- Externalized configuration via values.yaml
- Containerized microservices architecture
- Production-like networking with Ingress controller
- Different autoscaling strategies for dev and production environments (replica and CPU thresholds)
- GitOps workflow with ArgoCD (automated cluster synchronization from GitHub)

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
- Deployment is handled by ArgoCD via GitOps (pull-based model)

In production environments, deployment would be handled via Helm or GitOps (ArgoCD).

# GitOps (ArgoCD)
This project uses Argo CD for GitOps-based deployment.
ArgoCD continuously syncs the Kubernetes cluster with the Helm configuration stored in GitHub.
Any change pushed to the repository is automatically applied to the cluster (automated sync enabled).
This removes the need for manual deployments using kubectl or helm after initial setup.
It demonstrates a production-style pull-based deployment model (Git → Cluster).

# Tech Stack
- Kubernetes (kind)
- Helm
- Docker
- GitHub Actions
- NGINX Ingress Controller
- Node.js
- PostgreSQL (Bitnami Helm chart)
- Redis (Bitnami Helm chart)
- ArgoCD

# What I Learned
- Kubernetes deployment lifecycle
- Helm templating and environment management
- Service discovery and ingress routing
- CI pipelines for containerized applications
- Managing stateful services in Kubernetes
- Debugging ImagePullBackOff and deployment issues
- ArgoCD for continuos deployment

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
                        ┌──────▼────────┐
                        │   ArgoCD      │
                        │   (GitOps)    │
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