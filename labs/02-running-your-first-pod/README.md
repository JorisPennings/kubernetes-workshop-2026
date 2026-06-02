# Lab 2: Running Your First Pod

## Objective

Learn how to create and manage Pods — the smallest deployable unit in Kubernetes.

## Tasks

### 2.1 — Run a pod imperatively

```bash
kubectl run my-nginx --image=nginx:latest
```

Check that it's running:

```bash
kubectl get pods
```

### 2.2 — Inspect the pod

```bash
kubectl describe pod my-nginx
```

**Question:** What IP address was assigned to the pod? What node is it running on?

### 2.3 — View pod logs

```bash
kubectl logs my-nginx
```

### 2.4 — Execute a command inside the pod

```bash
kubectl exec -it my-nginx -- /bin/bash
```

Once inside, try:

```bash
curl localhost
exit
```

### 2.5 — Create a pod from a YAML manifest

Apply the provided manifest:

```bash
kubectl apply -f pod.yaml
```

Verify it's running:

```bash
kubectl get pods
```

### 2.6 — Clean up

```bash
kubectl delete pod my-nginx
kubectl delete -f pod.yaml
```

## Summary

You've learned how to:
- Create pods imperatively and declaratively
- Inspect, log, and exec into running pods
- Clean up resources

---

➡️ Continue to [Lab 3: Deployments and Scaling](../03-deployments-and-scaling/README.md)
