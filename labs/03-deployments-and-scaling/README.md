# Lab 3: Deployments and Scaling

## Objective

Learn how Deployments manage pods, enable scaling, and handle rolling updates.

## Tasks

### 3.1 - Create a Deployment

First, inspect the Deployment manifest (see [deployment.yaml](deployment.yaml)) to understand what it defines.

Then apply it:

```bash
kubectl apply -f deployment.yaml
```

Check the deployment and its pods:

```bash
kubectl get deployments
kubectl get pods
```

**Question:** How many pods are running?

### 3.2 - Scale the Deployment

```bash
kubectl scale deployment hello-deployment --replicas=5
```

Watch the pods come up:

```bash
kubectl get pods -w
```

(Press Ctrl+C to stop watching)

### 3.3 - Test self-healing

Delete one of the pods:

```bash
kubectl delete pod <pod-name>
```

Then immediately check:

```bash
kubectl get pods
```

**Question:** What happened? How many pods are running now?

### 3.4 - Rolling update

Update the image to a newer version:

```bash
kubectl set image deployment/hello-deployment hello=hashicorp/http-echo:0.2.3
```

Watch the rollout:

```bash
kubectl rollout status deployment/hello-deployment
```

### 3.5 - Trigger an ImagePullBackOff

Let's deliberately break the deployment to see how Kubernetes handles failures. Update to a non-existent image tag:

```bash
kubectl set image deployment/hello-deployment hello=hashicorp/http-echo:nonexistent
```

Check the status:

```bash
kubectl describe deployment hello-deployment
kubectl describe pod -l app=hello
```

**Question:** What's the status of the pods? Can you see the `ImagePullBackOff` error? What does it mean?

Now rollback to fix it:

```bash
kubectl rollout undo deployment/hello-deployment
```

Check the status again:

```bash
kubectl get pods
```

**Question:** Are the pods running again? What happened when you rolled back?

### 3.6 - Clean up

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
