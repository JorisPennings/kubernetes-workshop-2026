# Kubernetes Setup Instructions

Choose **one** of the options below to get a local Kubernetes cluster running on your machine.

---

## Option 1: Rancher Desktop (Recommended for beginners)

Rancher Desktop provides a GUI and bundles everything you need.

### Install

- **Linux:** Download the `.AppImage` from [rancherdesktop.io](https://rancherdesktop.io/) or install via your package manager
- **macOS:** `brew install --cask rancher`
- **Windows:** Download the installer from [rancherdesktop.io](https://rancherdesktop.io/)

### Setup

1. Launch Rancher Desktop
2. Select **containerd** as the container runtime
3. Choose a Kubernetes version (latest stable is fine)
4. Wait for the cluster to start (you'll see a green indicator)
5. `kubectl` is included — open a terminal and verify:

```bash
kubectl get nodes
```

---

## Option 2: Minikube

Lightweight and widely used for local development.

### Install

- **Linux:**
  ```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```
- **macOS:** `brew install minikube`
- **Windows:** `winget install minikube`

### Prerequisites

You need a container/VM driver. Docker is the easiest:

```bash
# Install Docker if you don't have it
# https://docs.docker.com/get-docker/
```

### Setup

```bash
# Start a cluster
minikube start

# Verify
kubectl get nodes

# Enable useful addons for the workshop
minikube addons enable ingress
minikube addons enable metrics-server
```

### Useful commands

```bash
minikube status          # Check cluster status
minikube dashboard       # Open the Kubernetes dashboard in browser
minikube stop            # Stop the cluster (preserves state)
minikube delete          # Delete the cluster entirely
```

---

## Option 3: kind (Kubernetes IN Docker)

Runs Kubernetes nodes as Docker containers. Fast startup, no VM needed.

### Install

- **Linux/macOS:**
  ```bash
  # Via Go
  go install sigs.k8s.io/kind@latest

  # Or download the binary
  curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
  chmod +x ./kind
  sudo mv ./kind /usr/local/bin/kind
  ```
- **macOS:** `brew install kind`
- **Windows:** `choco install kind`

### Prerequisites

Docker must be installed and running.

### Setup

```bash
# Create a cluster
kind create cluster --name workshop

# Verify
kubectl get nodes
```

### Delete when done

```bash
kind delete cluster --name workshop
```

---

## Option 4: Docker Desktop (macOS/Windows)

If you already have Docker Desktop, Kubernetes is built in.

1. Open Docker Desktop → **Settings** → **Kubernetes**
2. Check **Enable Kubernetes**
3. Click **Apply & Restart**
4. Wait for the Kubernetes indicator to turn green
5. Verify:

```bash
kubectl get nodes
```

---

## Installing kubectl separately

If your chosen tool doesn't bundle `kubectl`:

- **Linux:**
  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  chmod +x kubectl
  sudo mv kubectl /usr/local/bin/
  ```
- **macOS:** `brew install kubectl`
- **Windows:** `winget install Kubernetes.kubectl`

Verify:

```bash
kubectl version --client
```

---

## Verify your setup

Run these commands to confirm everything is ready for the workshop:

```bash
# Cluster is reachable
kubectl cluster-info

# At least one node is Ready
kubectl get nodes

# You can create resources
kubectl run test --image=nginx --restart=Never
kubectl get pods
kubectl delete pod test
```

If all commands succeed, you're good to go!
