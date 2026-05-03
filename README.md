# Kubernetes Platform Project (DevOps Showcase)

# Description
Production-like Kubernetes platform built with Helm, featuring microservices deployment, ingress routing, and CI pipeline for containerized applications.

# Features
- Kubernetes-based microservices deployment
- Helm chart with environment-based configuration
- NGINX Ingress for HTTP routing
- Redis and PostgreSQL integration
- CI pipeline for automated Docker builds
- Health checks (liveness/readiness probes)
- Resource limits and requests
- Secrets management via Kubernetes Secrets

# How to run locally
kind create cluster --config kind-config.yaml
kubectl apply -f ingress-nginx.yaml
helm upgrade --install my-app ./helm/my-app -f values-dev.yaml

# Access
http://my-app.local

# CI/CD note (IMPORTANT)
Note: Deployment step is disabled in CI as the Kubernetes cluster runs locally (kind).
In a production setup, this would deploy automatically via Helm.

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