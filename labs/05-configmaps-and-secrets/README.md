# Lab 5: ConfigMaps and Secrets

## Objective

Learn how to externalize configuration using ConfigMaps and sensitive data using Secrets.

## Tasks

### 5.1 - Create a ConfigMap

First, inspect the ConfigMap manifest (see [configmap.yaml](configmap.yaml)) to understand what configuration it defines.

Then apply it:

```bash
kubectl apply -f configmap.yaml
```

Inspect it:

```bash
kubectl describe configmap app-config
```

### 5.2 - Create a Secret

First, inspect the Secret manifest (see [secret.yaml](secret.yaml)) to see how sensitive data is defined.

Then apply it:

```bash
kubectl apply -f secret.yaml
```

Inspect it:

```bash
kubectl get secret app-secret -o yaml
```

**Question:** Are the values readable? How are they encoded?

### 5.3 - Deploy an app using the ConfigMap and Secret

Inspect the Deployment manifest (see [deployment.yaml](deployment.yaml)) to see how it references the ConfigMap and Secret.

Then deploy it:

```bash
kubectl apply -f deployment.yaml
```

Check the environment variables inside the pod.

**💡 kubectl exec hint:** `kubectl exec` runs a command inside a pod. The `-it` flags enable interactive mode (`-i`) and allocate a TTY (`-t`). The `--` separates kubectl arguments from the command to run inside the container.

**Question:** Can you see both the ConfigMap values and the Secret values?

### 5.4 - Mount ConfigMap as a file

Inspect the Deployment manifest (see [deployment-volume.yaml](deployment-volume.yaml)) to see how it mounts a ConfigMap as a volume.

Then apply it:

```bash
kubectl apply -f deployment-volume.yaml
```

Check the mounted file:

```bash
kubectl exec -it $(kubectl get pod -l app=config-volume-demo -o name | head -1) -- cat /etc/config/config.properties
```

### 5.5 - Clean up

```bash
kubectl delete -f configmap.yaml
kubectl delete -f secret.yaml
kubectl delete -f deployment.yaml
kubectl delete -f deployment-volume.yaml
```

## Summary

You've learned how to:
- Create ConfigMaps for non-sensitive configuration
- Create Secrets for sensitive data
- Inject config as environment variables
- Mount config as files in a container

---

➡️ Continue to [Lab 6: Persistent Storage](../06-persistent-storage/README.md)
