# Lab 3: Deployments and Scaling

## Objective

Learn how Deployments manage pods, enable scaling, and handle rolling updates.

## Tasks

### 3.1 — Create a Deployment

```bash
kubectl apply -f deployment.yaml
```

Check the deployment and its pods:

```bash
kubectl get deployments
kubectl get pods
```

**Question:** How many pods are running?

### 3.2 — Scale the Deployment

```bash
kubectl scale deployment hello-deployment --replicas=5
```

Watch the pods come up:

```bash
kubectl get pods -w
```

(Press Ctrl+C to stop watching)

### 3.3 — Test self-healing

Delete one of the pods:

```bash
kubectl delete pod <pod-name>
```

Then immediately check:

```bash
kubectl get pods
```

**Question:** What happened? How many pods are running now?

### 3.4 — Rolling update

Update the image:

```bash
kubectl set image deployment/hello-deployment hello=hashicorp/http-echo:latest -- 
```

Watch the rollout:

```bash
kubectl rollout status deployment/hello-deployment
```

### 3.5 — Rollback

```bash
kubectl rollout undo deployment/hello-deployment
```

Verify:

```bash
kubectl rollout status deployment/hello-deployment
```

### 3.6 — Clean up

```bash
kubectl delete -f deployment.yaml
```

## Summary

You've learned how to:
- Create Deployments that manage multiple pod replicas
- Scale up and down
- See self-healing in action
- Perform rolling updates and rollbacks

---

➡️ Continue to [Lab 4: Exposing Apps with Services](../04-services/README.md)
