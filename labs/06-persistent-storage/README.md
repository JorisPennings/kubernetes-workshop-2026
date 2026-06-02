# Lab 6: Persistent Storage

## Objective

Learn how to persist data using PersistentVolumeClaims so your data survives pod restarts.

## Tasks

### 6.1 — Check available StorageClasses

```bash
kubectl get storageclasses
```

**Question:** What StorageClass is available? Is there a default?

### 6.2 — Create a PersistentVolumeClaim

```bash
kubectl apply -f pvc.yaml
```

Check the status:

```bash
kubectl get pvc
```

**Question:** What is the status of the PVC? (It may be `Pending` until a pod uses it with local-path-storage.)

### 6.3 — Deploy a pod that writes to the volume

```bash
kubectl apply -f deployment-storage.yaml
```

Wait for it to be running:

```bash
kubectl get pods -l app=storage-demo -w
```

Write some data:

```bash
kubectl exec -it $(kubectl get pod -l app=storage-demo -o name | head -1) -- sh -c "echo 'Hello from persistent storage!' > /data/hello.txt"
```

Verify:

```bash
kubectl exec -it $(kubectl get pod -l app=storage-demo -o name | head -1) -- cat /data/hello.txt
```

### 6.4 — Test persistence across pod restarts

Delete the pod (the Deployment will recreate it):

```bash
kubectl delete pod -l app=storage-demo
```

Wait for the new pod to come up:

```bash
kubectl get pods -l app=storage-demo -w
```

Check if the data survived:

```bash
kubectl exec -it $(kubectl get pod -l app=storage-demo -o name | head -1) -- cat /data/hello.txt
```

**Question:** Is the file still there? Why?

### 6.5 — Inspect the PersistentVolume

```bash
kubectl get pv
kubectl describe pv $(kubectl get pv -o name | head -1)
```

**Question:** Where on the node is the data actually stored?

### 6.6 — Clean up

```bash
kubectl delete -f deployment-storage.yaml
kubectl delete -f pvc.yaml
```

## Summary

You've learned how to:
- Use StorageClasses to dynamically provision volumes
- Create PersistentVolumeClaims to request storage
- Mount volumes into pods
- Verify that data persists across pod restarts

---

➡️ Continue to [Lab 7: Exposing Apps with Ingress](../07-ingress/README.md)
