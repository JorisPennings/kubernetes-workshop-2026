# Lab 7: Exposing Apps with Ingress

## Objective

Learn how to use an Ingress resource to route external HTTP traffic to your services, a more production-like approach than NodePort.

## Prerequisites

Make sure you have an Ingress controller installed.

**For minikube:**

```bash
minikube addons enable ingress
```

**For Rancher Desktop:**

Rancher Desktop comes with **Traefik** pre-installed as the default Ingress controller, so you don't need to install anything! You can verify it's running:

```bash
kubectl get pods -n kube-system | grep traefik
```

**For other platforms (or if you prefer NGINX):**

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/cloud/deploy.yaml
```

Verify the controller is running:

```bash
kubectl get pods -n ingress-nginx
```

## Tasks

### 7.1 - Deploy two apps

We'll deploy two simple apps and route to them based on the URL path.

First, inspect both app manifests (see [app-hello.yaml](app-hello.yaml) and [app-world.yaml](app-world.yaml)).

Then apply them:

```bash
kubectl apply -f app-hello.yaml
kubectl apply -f app-world.yaml
```

Verify both services are running:

```bash
kubectl get deployments
kubectl get services
```

### 7.2 - Create an Ingress resource

First, inspect the Ingress manifest (see [ingress.yaml](ingress.yaml)) to understand how it routes traffic.

Then apply it:

```bash
kubectl apply -f ingress.yaml
```

Inspect it:

```bash
kubectl get ingress
kubectl describe ingress demo-ingress
```

**Question:** What host and paths are configured?

### 7.3 - Set up port forwarding

To access the Ingress on localhost:8080, you need to port-forward the Ingress controller service:

**For Rancher Desktop (Traefik):**

```bash
kubectl port-forward -n kube-system svc/traefik 8080:80
```

**For minikube (NGINX):**

```bash
kubectl port-forward -n ingress-nginx svc/ingress-nginx-controller 8080:80
```

Open a new terminal, while the other one runs the port-forward.

**Note:** Port forwarding setup may vary depending on your Kubernetes distribution. The service name and namespace differ between platforms. If the command fails, find the correct service with `kubectl get svc -A` and adjust accordingly.

### 7.4 - Test the Ingress

Test the routes using localhost on port 8080:

```bash
curl http://localhost:8080/hello
curl http://localhost:8080/world
```

**Question:** Do you get different responses for each path?

### 7.5 - Bonus: Check the Ingress details

```bash
kubectl describe ingress demo-ingress
```

**Question:** What is the ADDRESS shown? How is it different between minikube and Rancher Desktop?

### 7.6 - Clean up

Stop the port-forward:

```bash
Ctrl + c
```

Delete the resources:

```bash
kubectl delete -f ingress.yaml
kubectl delete -f app-hello.yaml
kubectl delete -f app-world.yaml
```

## Summary

You've learned how to:
- Set up an Ingress controller
- Create Ingress resources with path-based routing
- Route traffic to different services based on URL path
- Understand the role of the Host header in Ingress routing

---

🎉 **Congratulations!** You've completed all the Basic labs!
