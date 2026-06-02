# Lab 7: Exposing Apps with Ingress

## Objective

Learn how to use an Ingress resource to route external HTTP traffic to your services — a more production-like approach than NodePort.

## Prerequisites

Make sure you have an Ingress controller installed. For minikube:

```bash
minikube addons enable ingress
```

Verify the controller is running:

```bash
kubectl get pods -n ingress-nginx
```

## Tasks

### 7.1 — Deploy two apps

We'll deploy two simple apps and route to them based on the URL path.

```bash
kubectl apply -f app-hello.yaml
kubectl apply -f app-world.yaml
```

Verify both services are running:

```bash
kubectl get deployments
kubectl get services
```

### 7.2 — Create an Ingress resource

```bash
kubectl apply -f ingress.yaml
```

Inspect it:

```bash
kubectl get ingress
kubectl describe ingress demo-ingress
```

**Question:** What host and paths are configured?

### 7.3 — Test the Ingress

Get the Ingress address (minikube):

```bash
minikube ip
```

Add a hosts entry (or use the IP directly):

```bash
echo "$(minikube ip) demo.local" | sudo tee -a /etc/hosts
```

Test the routes:

```bash
curl http://demo.local/hello
curl http://demo.local/world
```

**Question:** Do you get different responses for each path?

### 7.4 — Bonus: try without the host entry

```bash
curl http://$(minikube ip)/hello -H "Host: demo.local"
```

**Question:** Why does the `-H "Host: ..."` header matter?

### 7.5 — Clean up

```bash
kubectl delete -f ingress.yaml
kubectl delete -f app-hello.yaml
kubectl delete -f app-world.yaml
```

Remove the hosts entry if you added one:

```bash
sudo sed -i '/demo.local/d' /etc/hosts
```

## Summary

You've learned how to:
- Set up an Ingress controller
- Create Ingress resources with path-based routing
- Route traffic to different services based on URL path
- Understand the role of the Host header in Ingress routing

---

🎉 **Congratulations!** You've completed all the labs!
