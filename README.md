# Kubernetes Minimal Demo: Pod Communication & Host-Service

## Overview

This repository demonstrates a **minimal Kubernetes setup** for learning purposes:

- **Pod1** and **Pod2** communicate inside a cluster.
- **Host-Service** simulates an external service or host, allowing pods to communicate with a "host" (local machine or another service).
- Access the pods/services from your local machine using **NodePort** or **port-forwarding**.
- All Kubernetes resources are defined in **separate YAML files** for modularity.

> ⚠️ This setup is for **local development/testing** only.

---

## Repository Structure

```text
k8s-demo/
├── host-pod.yaml           # Pod simulating a host/external service
├── host-service-svc.yaml   # NodePort Service for host pod
├── pod1.yaml               # Frontend Pod
├── pod1-service.yaml       # NodePort Service for Pod1
├── pod2.yaml               # Backend Pod
├── README.md               # This file
└── .gitignore              # Ignore temp/log files
```

---

## 1️⃣ Purpose of This Repo

This repo demonstrates minimal Kubernetes pod communication for learning and local development:

- **Pod2** communicates with **Pod1** (ClusterIP service)
- **Pod2** communicates with **Host-Service** (simulates host/external service)
- **Windows host** can access Pod1 or Host-Service using NodePort or port-forward

> ⚠️ **Note:** This setup is simplified for local development and testing.
> It is not production-ready, but it avoids complex configs and overhead.

---

## 2️⃣ Why This Approach Was Chosen

| Feature / Setup                          | Reasoning                                                                 |
|------------------------------------------|---------------------------------------------------------------------------|
| Separate YAML files for each pod/service | Easier to understand and modify, minimal overhead for learners.           |
| Host-Service Pod + NodePort              | Simulates host/external service communication without relying on host IP. |
| NodePort + port-forward                  | Provides simple access from Windows host for testing/debugging.           |
| ClusterIP for Pod1 → Pod2                | Correct way for pod-to-pod communication inside Kubernetes cluster.       |
| Avoid hardcoding host IPs                | Ensures portability across machines and environments.                     |

> Essentially: We prioritize **simplicity**, **clarity**, and **hands-on testing** over production best practices.

---

## 3️⃣ Architecture Diagram

```text
[External System / API / Host Service]
        ↑
        |  (DNS / NodePort / port-forward)
        ↓
[Kubernetes Service] <---> [Pod]
```

---

## 4️⃣ Deployment (Bare Minimum)

```bash
kubectl create ns test

kubectl apply -f pod1.yaml
kubectl apply -f pod1-service.yaml
kubectl apply -f pod2.yaml
kubectl apply -f host-pod.yaml
kubectl apply -f host-service.yaml
```

---

## 5️⃣ Testing

### Inside Cluster (Pod2)

```bash
kubectl exec -it pod2 -n test -- sh

# Pod2 → Pod1
curl pod1-service:80

# Pod2 → Host-Service
curl host-service:8080
```

### From Windows Host

```bash
# Check NodePorts
kubectl get svc -n test

# Access Pod1
curl http://localhost:<pod1-nodeport>

# Access Host-Service
curl http://localhost:<host-service-nodeport>

# Alternative: port-forward for guaranteed access
kubectl port-forward svc/host-service 8080:8080 -n test
curl http://localhost:8080
```

---

## 6️⃣ Debugging Commands

```bash
# Check all pods and their labels
kubectl get pods -n test --show-labels

# Check all services
kubectl get svc -n test

# View logs
kubectl logs pod1 -n test
kubectl logs host-service -n test

# Shell into Pod2 and test connectivity
kubectl exec -it pod2 -n test -- sh
curl pod1-service:80
curl host-service:8080

# Port-forward for direct access
kubectl port-forward svc/host-service 8080:8080 -n test
```
