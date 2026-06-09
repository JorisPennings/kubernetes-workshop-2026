# Kubernetes Workshop 2026

A hands-on beginner workshop to get familiar with Kubernetes fundamentals.

## Prerequisites

- A running Kubernetes cluster - see [SETUP.md](SETUP.md) for installation options
- `kubectl` installed and configured
- Basic terminal/command-line experience

## Workshop Structure

| Time | Activity |
|------|----------|
| 15 min | Presentation: What is Kubernetes and why? |
| 10 min | Lab 1: Exploring the cluster |
| 15 min | Lab 2: Running your first Pod |
| 15 min | Lab 3: Deployments and scaling |
| 15 min | Lab 4: Exposing apps with Services |
| 15 min | Lab 5: Configuration with ConfigMaps and Secrets |
| 15 min | Lab 6: Persistent Storage |
| 15 min | Lab 7: Exposing apps with Ingress |
| 5 min | Wrap-up and Q&A |

## Getting Started

```bash
# Verify your cluster is running
kubectl cluster-info

# Verify kubectl works
kubectl get nodes
```

Then proceed to [Lab 1](labs/01-exploring-the-cluster/README.md).

## Bonus: Challenge Yourself

Finished all the labs? Try these open-ended challenges - no step-by-step commands, just requirements and hints:

| Challenge | Description |
|-----------|-------------|
| [Challenge 1: Guestbook](labs-challenge-yourself/challenge-01-guestbook/README.md) | Deploy a multi-tier app (Redis + frontend) from scratch |
| [Challenge 2: Fix the Broken Deployment](labs-challenge-yourself/challenge-02-fix-the-broken-deployment/README.md) | Debug intentionally broken manifests |
| [Challenge 3: Blue-Green Deployment](labs-challenge-yourself/challenge-03-blue-green/README.md) | Zero-downtime traffic switching between app versions |
