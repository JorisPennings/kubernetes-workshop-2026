# Lab 4: Exposing Apps with Services

## Objective

Learn how Services provide stable networking to access your pods.

## Tasks

### 4.1 - Deploy the app

```bash
kubectl apply -f deployment.yaml
```

### 4.2 - Create a ClusterIP Service

First, inspect the Service manifest (see [service-clusterip.yaml](service-clusterip.yaml)) to understand how it's configured.

Then apply it:

```bash
kubectl apply -f service-clusterip.yaml
```

Check the service:

```bash
kubectl get services
```

**Question:** What ClusterIP was assigned?

### 4.3 - Test internal access

Start a temporary pod with curl to test the service:

```bash
kubectl run curl-test --image=alpine:latest --rm -it -- sh -c "apk add curl && curl http://hello-service:5678"
```

**Question:** What response do you get?

### 4.4 - Create a NodePort Service

First, inspect the Service manifest (see [service-nodeport.yaml](service-nodeport.yaml)) to understand how it differs from the ClusterIP service.

Then apply it:

```bash
kubectl apply -f service-nodeport.yaml
```

Check the assigned port:

```bash
kubectl get service hello-nodeport
```

curl it from you PC

```bash
curl http://<node-ip>:<node-port>
```

### 4.5 - Clean up

```bash
kubectl delete -f deployment.yaml
kubectl delete -f service-clusterip.yaml
kubectl delete -f service-nodeport.yaml
```

## Summary

You've learned how to:
- Expose pods internally with ClusterIP services
- Expose pods externally with NodePort services
- Use DNS-based service discovery inside the cluster

---

➡️ Continue to [Lab 5: ConfigMaps and Secrets](../05-configmaps-and-secrets/README.md)
