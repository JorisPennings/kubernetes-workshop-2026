# Lab 1: Exploring the Cluster

## Objective

Get comfortable with `kubectl` and understand the basic building blocks of a Kubernetes cluster.

## Tasks

### 1.1 - Check cluster info

```bash
kubectl cluster-info
```

**Question:** What is the address of the Kubernetes control plane?

### 1.2 - List nodes

```bash
kubectl get nodes
```

**Question:** How many nodes does your cluster have? What are their roles?

### 1.3 - Inspect a node

```bash
kubectl describe node <node-name>
```

**Question:** How much CPU and memory is allocatable on your node?

### 1.4 - Explore namespaces

```bash
kubectl get namespaces
```

**Question:** What namespaces exist by default? What do you think they're used for?

### 1.5 - Look at running system pods

```bash
kubectl get pods -n kube-system
```

**Question:** Can you identify what each pod is responsible for?

## Summary

You've learned how to:
- Connect to a cluster with `kubectl`
- Inspect nodes and their resources
- Understand namespaces as a way to organize resources
- See the system components that make Kubernetes work

---

➡️ Continue to [Lab 2: Running Your First Pod](../02-running-your-first-pod/README.md)
