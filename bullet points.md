## 1. What is Kubernetes and Why It Exists

### Problem Kubernetes Solves

Modern applications:

* Are built as **microservices**
* Run in **containers**
* Need **high availability**, **scaling**, **self-healing**, and **automation**

Manually managing containers across many machines is not feasible.

### Purpose of Kubernetes

Kubernetes is a **container orchestration platform** that:

* Deploys applications
* Scales them automatically
* Restarts failed components
* Manages networking and storage
* Provides declarative configuration

---

## 2. Kubernetes Cluster Architecture

A Kubernetes cluster consists of **Control Plane** and **Worker Nodes**.

---

## 3. Control Plane Components (Brain of the Cluster)

### 3.1 API Server

**Purpose**

* Entry point to Kubernetes
* All commands (`kubectl`, CI/CD, UI) go through API Server

**Responsibilities**

* Validates requests
* Persists state to etcd
* Acts as a REST API

Without API Server → cluster cannot be managed.

---

### 3.2 etcd

**Purpose**

* Distributed key-value database
* Stores the **entire cluster state**

**Stores**

* Pods
* Secrets
* ConfigMaps
* Node info
* RBAC rules

**Why critical**

* If etcd is lost → cluster state is lost
* Needs backup and high availability

---

### 3.3 Scheduler

**Purpose**

* Decides **which node** a Pod should run on

**Considers**

* CPU and memory availability
* Node selectors
* Taints and tolerations
* Affinity / anti-affinity rules

It does NOT create Pods — only assigns nodes.

---

### 3.4 Controller Manager

**Purpose**

* Ensures desired state = actual state

**Controllers include**

* Node Controller (node health)
* ReplicaSet Controller (replica count)
* Job Controller
* Endpoint Controller

Example:
If desired replicas = 3 and one Pod dies → controller creates a new one.

---

## 4. Worker Node Components (Execution Layer)

---

### 4.1 kubelet

**Purpose**

* Agent running on each worker node

**Responsibilities**

* Talks to API Server
* Starts containers via runtime
* Reports node and Pod health

Without kubelet → node cannot run Pods.

---

### 4.2 Container Runtime

**Purpose**

* Actually runs containers

**Examples**

* containerd
* CRI-O

Kubernetes does not run containers directly.

---

### 4.3 kube-proxy

**Purpose**

* Handles **networking rules**
* Enables Services and load balancing

**Works using**

* iptables / IPVS

Ensures traffic reaches the correct Pod.

---

## 5. Kubernetes Objects (Building Blocks)

---

## 6. Pod

### Purpose

* **Smallest deployable unit**
* Wraps one or more containers

### Key Points

* Containers in a Pod share:

  * Network namespace
  * IP address
  * Volumes

Pods are **ephemeral**:

* If a Pod dies, it is not repaired
* Higher-level objects manage Pods

---

## 7. ReplicaSet

### Purpose

* Ensures a fixed number of Pods are running

Example:

* Desired replicas: 3
* If one Pod crashes → new Pod created

Usually not created directly.

---

## 8. Deployment

### Purpose

* Manages ReplicaSets
* Provides **rolling updates** and **rollbacks**

### What it enables

* Zero-downtime deployments
* Version control of releases
* Easy rollback

Most common object for stateless apps.

---

## 9. StatefulSet

### Purpose

* Manages **stateful applications**

### Key Features

* Stable Pod names (pod-0, pod-1)
* Stable storage (PVC per Pod)
* Ordered startup and shutdown

Used for:

* Databases
* Kafka
* Elasticsearch

---

## 10. DaemonSet

### Purpose

* Runs **one Pod per node**

Use cases:

* Log collectors
* Monitoring agents
* Network plugins

Automatically runs on new nodes.

---

## 11. Job and CronJob

### Job

**Purpose**

* Run a task to completion

Examples:

* Database migration
* One-time batch processing

### CronJob

**Purpose**

* Scheduled Jobs (like Linux cron)

Examples:

* Nightly backups
* Periodic reports

---

## 12. Services (Networking Abstraction)

### Problem

Pods are ephemeral and IPs change.

### Purpose of Service

* Provides a **stable network endpoint**

---

### Types of Services

#### ClusterIP

* Internal-only access
* Default service type

#### NodePort

* Exposes service on node IP + port
* Mostly for testing

#### LoadBalancer

* Cloud provider creates external LB
* Production external access

#### Headless Service

* No load balancing
* Used by StatefulSets

---

## 13. Ingress

### Purpose

* HTTP/HTTPS routing
* URL-based traffic control

### Features

* SSL termination
* Path-based routing
* Host-based routing

Ingress requires an **Ingress Controller** (NGINX, Traefik).

---

## 14. Configuration Management

---

### ConfigMap

**Purpose**

* Store non-sensitive config data

Used for:

* Application configs
* Environment variables

---

### Secret

**Purpose**

* Store sensitive data securely

Used for:

* Passwords
* Tokens
* Certificates

Secrets are base64 encoded and RBAC protected.

---

## 15. Storage in Kubernetes

---

### Volume

* Exists only for Pod lifetime

---

### PersistentVolume (PV)

**Purpose**

* Cluster-level storage resource

Provisioned by:

* Admin (static)
* Storage class (dynamic)

---

### PersistentVolumeClaim (PVC)

**Purpose**

* Request storage by an application

Decouples app from storage backend.

---

### StorageClass

**Purpose**

* Defines how storage is provisioned

Example:

* EBS
* Azure Disk
* NFS

---

## 16. Scaling and Autoscaling

---

### Horizontal Pod Autoscaler (HPA)

**Purpose**

* Scales Pods based on metrics

Metrics:

* CPU
* Memory
* Custom metrics

---

### Vertical Pod Autoscaler (VPA)

**Purpose**

* Adjusts resource requests

Restarts Pods when resizing.

---

### Cluster Autoscaler

**Purpose**

* Adds or removes nodes automatically

---

## 17. Resource Management

---

### Requests

* Minimum resources guaranteed

### Limits

* Maximum resources allowed

Prevents noisy neighbor problems.

---

## 18. Security in Kubernetes

---

### Namespaces

**Purpose**

* Logical isolation

Used for:

* Multi-team clusters
* Environment separation

---

### RBAC

**Purpose**

* Access control

Defines:

* Who can do what
* On which resources

---

### Network Policies

**Purpose**

* Control Pod-to-Pod traffic

Default: all traffic allowed
Policies restrict traffic.

---

### Pod Security Standards

**Purpose**

* Enforce secure Pod behavior

Levels:

* Privileged
* Baseline
* Restricted

---

## 19. Observability

---

### Logging

* `kubectl logs`
* Centralized logging (ELK, Loki)

---

### Monitoring

* Metrics Server
* Prometheus
* Grafana

---

### Tracing

* Jaeger
* OpenTelemetry

---

## 20. Kubernetes Lifecycle Summary

1. User applies YAML
2. API Server validates
3. etcd stores state
4. Scheduler assigns node
5. kubelet runs Pod
6. Controller monitors health
7. kube-proxy routes traffic

---

## 21. Why Kubernetes is Used in Production

* Self-healing
* High availability
* Declarative management
* Cloud portability
* Scalability
* Automation
