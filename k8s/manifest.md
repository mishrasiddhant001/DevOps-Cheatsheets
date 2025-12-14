# Kubernetes Manifest — Definition and Purpose

## What is a Kubernetes Manifest?

A **Kubernetes manifest** is typically a **YAML (or JSON) file** that defines:

* **Which Kubernetes resource** you want
* **How it should look**
* **What state Kubernetes must maintain**

In simple terms:

> A manifest tells Kubernetes **“this is the desired state — make it happen and keep it that way.”**

---

## Why Manifests Exist (Purpose)

Manifests allow you to:

* Define infrastructure **as code**
* Version-control deployments (Git)
* Ensure **consistency** across environments
* Enable **automation and CI/CD**
* Avoid manual configuration drift

Kubernetes continuously reconciles the cluster to match what is defined in the manifest.

---

## What a Manifest Contains

Every Kubernetes manifest has **four mandatory sections**:

```yaml
apiVersion: apps/v1        # Which API version to use
kind: Deployment           # Type of resource
metadata:                 # Identity information
  name: my-app
  labels:
    app: my-app
spec:                     # Desired state definition
  replicas: 3
```

### 1. apiVersion

**Purpose**

* Tells Kubernetes which API version the resource belongs to

Examples:

* `v1` → Pod, Service
* `apps/v1` → Deployment, StatefulSet
* `batch/v1` → Job, CronJob

---

### 2. kind

**Purpose**

* Specifies the type of Kubernetes object

Examples:

* Pod
* Deployment
* Service
* ConfigMap
* Secret

---

### 3. metadata

**Purpose**

* Identifies and organizes the resource

Includes:

* `name` (must be unique in namespace)
* `namespace`
* `labels`
* `annotations`

Used for:

* Selection
* Grouping
* Automation
* Monitoring

---

### 4. spec

**Purpose**

* Describes the **desired state**

Examples:

* Number of replicas
* Container image
* Ports
* Environment variables
* Volumes

Kubernetes works continuously to match reality with `spec`.

---

## Example: Simple Pod Manifest

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.25
    ports:
    - containerPort: 80
```

**Purpose of this manifest**

* Run one Pod
* Using nginx image
* Expose port 80 inside the Pod

---

## Example: Deployment Manifest (Production Use)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.25
```

**Purpose**

* Maintain 3 replicas
* Automatically replace failed Pods
* Enable rolling updates

---

## How Manifests Are Used

### Apply a Manifest

```bash
kubectl apply -f deployment.yaml
```

### What Happens Internally

1. Manifest sent to API Server
2. Validated against schema
3. Stored in etcd
4. Controllers enforce desired state

---

## Types of Manifests

| Manifest Type | Purpose                   |
| ------------- | ------------------------- |
| Pod           | Single container workload |
| Deployment    | Stateless applications    |
| StatefulSet   | Stateful apps             |
| Service       | Networking                |
| Ingress       | HTTP routing              |
| ConfigMap     | Configuration             |
| Secret        | Sensitive data            |
| PVC           | Persistent storage        |
| RBAC          | Security permissions      |

---

## Declarative vs Imperative

### Manifest (Declarative)

```bash
kubectl apply -f app.yaml
```

* Preferred
* Repeatable
* Version-controlled

### Command (Imperative)

```bash
kubectl run nginx --image=nginx
```

* One-time
* Not reproducible

---

## Manifest Best Practices

* One resource per file (recommended)
* Use labels consistently
* Store in Git
* Avoid hardcoding secrets
* Validate with `kubectl apply --dry-run=client`
