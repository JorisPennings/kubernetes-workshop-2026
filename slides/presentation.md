---
marp: true
theme: default
paginate: true
header: "Kubernetes Workshop 2026"
footer: ""
style: |
  section {
    font-size: 28px;
  }
  h1 {
    color: #326CE5;
  }
  h2 {
    color: #326CE5;
  }
  code {
    background: #f0f0f0;
    padding: 2px 6px;
    border-radius: 4px;
  }
  img[alt~="center"] {
    display: block;
    margin: 0 auto;
  }
  .two-columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    align-items: center;
  }
  pre code {
    font-size: 0.75em !important;
  }
---

# Kubernetes Fundamentals

### Een introductie in container orchestration

<br>
<br>

![height:200px center](https://raw.githubusercontent.com/kubernetes/kubernetes/master/logo/logo.svg)

---

## Welk probleem lost Kubernetes op?

Zonder orchestration:

- "Het werkt op mijn machine" 🤷
- Handmatig deployen, handmatig schalen
- Eén server valt uit → je applicatie ligt eruit
- Updaten van een app betekent downtime

**Kubernetes automatiseert dit allemaal.**

---

## Wat is Kubernetes?

- Open-source **container orchestration** platform
- Oorspronkelijk ontworpen door Google, nu onderhouden door de CNCF
- Vaak afgekort als **K8s** (K + 8 letters + s)

### Het regelt:

| Onderwerp | K8s Oplossing |
|-----------|--------------|
| Containers draaien | Pods |
| Schalen | Deployments + ReplicaSets |
| Netwerk | Services + Ingress |
| Configuratie | ConfigMaps + Secrets |
| Opslag | PersistentVolumes |

---

## Containers: Even opfrissen

Een container verpakt je applicatie + alle dependencies in één draagbare eenheid.

<div class="two-columns">
<div>

- Gebouwd met Docker, Podman, etc.
- Draait overal hetzelfde
- Lichtgewicht vergeleken met VM's

</div>
<div>

![height:280px](https://akfpartners.com/uploads/blog/VM_Image.PNG)

</div>
</div>

---

## Kubernetes Architectuur - Overzicht

![height:480px center](https://kubernetes.io/images/docs/kubernetes-cluster-architecture.svg)

---

## Control Plane

| Component | Rol |
|-----------|-----|
| **API Server** | User facing, alles gaat via de API |
| **etcd** | Database die de cluster state opslaat |
| **Scheduler** | Bepaalt op welke node een pod draait |
| **Controller Manager** | Bewaakt: gewenste state == werkelijke state |

### Hoe werken ze samen?

1. Je stuurt YAML naar de **API Server** (`kubectl apply`)
2. API Server slaat het op in **etcd**
3. **Scheduler**: "pod zonder node" → wijst er één toe
4. **Controller Manager**: controleert continu of alles klopt

---

## Worker Nodes

De machines waar je applicaties daadwerkelijk draaien.

| Component | Rol |
|-----------|-----|
| **kubelet** | Agent op elke node, start en stopt containers |
| **kube-proxy** | Netwerkregels voor pods en services |
| **Container Runtime** | Draait containers (containerd, CRI-O) |

```
Worker Node
├── kubelet          → "draai 3 nginx pods"
│   ├── Pod (nginx)  → container draait
│   ├── Pod (nginx)  → container draait
│   └── Pod (nginx)  → container draait
└── kube-proxy       → routeert verkeer naar pods
```

- Nodes melden zich aan bij het Control Plane
- kubelet rapporteert continu de pod-status terug

---

## Control Plane vs. Workers

```
  Jij (kubectl)
     │
     ▼
┌─────────────┐     "Ik wil 3 nginx pods"
│  API Server │ ◄── opgeslagen in etcd
└──────┬──────┘
       │
  ┌────┴────┐
  ▼         ▼
Scheduler   Controller Manager
  │         │
  │         └── "Er draaien er maar 2 → start er 1 bij"
  │
  └── "Pod naar Node 2 sturen"
       │
       ▼
  ┌─────────┐
  │ kubelet  │  → start de container
  │ (Node 2) │
  └─────────┘
```

---

## Kernconcepten - Overzicht

We gaan de volgende bouwstenen behandelen:

| Concept | Wat het doet |
|---------|-------------|
| **Pod** | Draait je container(s) |
| **Deployment** | Beheert meerdere pods, schalen & updates |
| **Service** | Stabiel netwerk adres voor pods |
| **Ingress** | HTTP routing van buiten het cluster |
| **ConfigMap / Secret** | Configuratie losgekoppeld van je app |
| **PersistentVolume** | Blijvende opslag voor data |

We gaan ze één voor één bekijken 👇

---

## Kernconcepten: Pods

De **kleinste deploybare eenheid** in Kubernetes.

- Omvat één of meer containers
- Containers delen netwerk en opslag
- Meestal: **1 pod = 1 container**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app
spec:
  containers:
    - name: app
      image: nginx:latest
```

---

## Kernconcepten: Deployments

Beheert identieke pods - regelt **schalen** en **updates**.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata: { name: my-app }
spec:
  replicas: 3
  selector: { matchLabels: { app: my-app } }
  template:
    spec:
      containers:
        - name: app
          image: nginx:latest
```

- Schalen door `replicas` aan te passen
- Rolling updates zonder downtime
- Self-healing: gecrashte pods worden vervangen

---

## Kernconcepten: Services

Pods krijgen willekeurige IP-adressen die veranderen. Een **Service** geeft ze een stabiel adres.

<div class="two-columns">
<div>

```
    ┌────────────┐
    │  Service   │
    │ 10.0.0.100 │
    └──────┬─────┘
┌──────┬──┴──┬──────┐
▼      ▼     ▼
Pod 1  Pod 2 Pod 3
```

</div>
<div>

| Type | Toegang |
|------|---------|
| **ClusterIP** | Alleen intern (standaard) |
| **NodePort** | Extern via node IP + poort |
| **LoadBalancer** | Extern via cloud load balancer |
</div>
</div>

---

## Kernconcepten: Ingress

Routeert **HTTP/HTTPS verkeer** van buiten naar je services.

```
  Internet
     │
     ▼
┌──────────┐
│  Ingress │  /app  → app-service
│Controller│  /api  → api-service
└──────────┘
```

- Routing op basis van pad of hostnaam
- TLS terminatie
- Eén ingangspunt, meerdere services

---

## Kernconcepten: ConfigMaps & Secrets

Configuratie **losgekoppeld** van je container image.

```yaml
# ConfigMap - niet-gevoelige configuratie
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: "blue"
  APP_MODE: "production"
```

```yaml
# Secret - gevoelige data (base64 encoded)
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_PASSWORD: c3VwZXJzZWNyZXQ=
```

- Injecteer als **environment variables** of **gemounte bestanden**
- Wijzig config zonder je image opnieuw te bouwen

---

## Kernconcepten: PersistentVolumes

Pods zijn **tijdelijk** - als een pod stopt, is de data weg. PersistentVolumes lossen dit op.

```
┌────────────────┐
│      Pod       │
│  ┌──────────┐  │      ┌──────────────────┐
│  │ container │──┼──────│ PersistentVolume │
│  └──────────┘  │      │  (opslag op disk) │
└────────────────┘      └──────────────────┘
```

- **PersistentVolumeClaim (PVC)**: je vraagt opslag aan
- **StorageClass**: bepaalt het type opslag (bv. `local-path`)
- Data blijft bestaan, ook als de pod herstart

---

## Het Declaratieve Model

Je vertelt Kubernetes niet **wat het moet doen**.
Je vertelt het **wat je wilt** - het regelt de rest.

```bash
# Imperatief (doe dit)
kubectl scale deployment my-app --replicas=5

# Declaratief (ik wil dit) ✅
# In je YAML: replicas: 5
kubectl apply -f deployment.yaml
```

Kubernetes **reconcileert continu**:
gewenste state ≠ werkelijke state → actie ondernemen

---

## kubectl: Hoe praat je met je cluster?

```bash
# Resources bekijken
kubectl get pods
kubectl get deployments
kubectl get services

# Aanmaken / updaten
kubectl apply -f my-file.yaml

# Inspecteren
kubectl describe pod my-app
kubectl logs my-app

# Debuggen
kubectl exec -it my-app -- /bin/bash

# Verwijderen
kubectl delete -f my-file.yaml
```

---

## Alles is YAML

Alle Kubernetes resources volgen dezelfde structuur:

```yaml
apiVersion: v1              # API versie
kind: Pod                   # Resource type
metadata:
  name: my-app              # Naam
  labels:
    app: my-app             # Labels voor selectie
spec:
  ...                       # De daadwerkelijke specificatie
```

- **Labels** → hoe resources elkaar vinden
- **Selectors** → hoe je filtert op labels
- `kubectl apply -f file.yaml` → aanmaken of updaten

---

## Samenvatting

| Concept | Wat het doet |
|---------|-------------|
| **Pod** | Draait je container(s) |
| **Deployment** | Beheert pod replicas, schalen, updates |
| **Service** | Stabiel netwerk adres voor pods |
| **Ingress** | HTTP routing van buiten |
| **ConfigMap** | Niet-gevoelige configuratie |
| **Secret** | Gevoelige configuratie |
| **PersistentVolume** | Blijvende opslag |

---

# Aan de slag!

### Tijd voor de labs 🚀

1. Controleer of je cluster draait (`kubectl get nodes`)
2. Open **Lab 1: Exploring the Cluster**

```bash
kubectl cluster-info
```

**Stel gerust vragen!**
