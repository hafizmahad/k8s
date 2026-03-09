# Kubernetes Minimal Demo: Pod Communication & Host-Service

[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.28-blue?logo=kubernetes)](https://kubernetes.io/)
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE)

## Overview

This repository demonstrates a **minimal Kubernetes setup** for learning purposes:

- **Pod1** and **Pod2** communicate inside a cluster.
- **Host-Service** simulates an external service or host, allowing pods to communicate with a "host" (local machine or another service).
- Access the pods/services from your local machine using **NodePort** or **port-forwarding**.
- All Kubernetes resources are defined in **separate YAML files** for modularity.

> ⚠️ This setup is for **local development/testing only**.

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




Purpose

This repository demonstrates minimal Kubernetes pod communication:

Pod2 communicates with Pod1 (via ClusterIP service)

Pod2 communicates with Host-Service (simulates an external host/service)

Local machine can access Pod1 or Host-Service using NodePort or port-forward

⚠️ Note: This setup is not production-ready but is ideal for learning and testing.



Why This Approach Was Chosen
Feature / Setup	Reasoning
Separate YAML files for each pod/service	Easier to understand and modify, minimal overhead for learners.
Host-Service Pod + NodePort	Simulates host/external service communication without relying on host IP.
NodePort + port-forward	Provides simple access from your local machine for testing/debugging.
ClusterIP for Pod1 → Pod2	Correct approach for pod-to-pod communication inside the Kubernetes cluster.
Avoid hardcoding host IPs	Ensures portability across machines and environments.

The focus is on simplicity, clarity, and hands-on testing over production best practices.


Architecture Overview
[External System / API / Host Service]
        ↑
        |  (DNS / NodePort / port-forward)
        ↓
[Kubernetes Service] <---> [Pod]

Pod1 and Pod2 communicate via a ClusterIP service.

Host-Service simulates external communication using a NodePort service.

The local machine can access services using NodePort or kubectl port-forward.

Purpose

This repository demonstrates minimal Kubernetes pod communication:

Pod2 communicates with Pod1 (via ClusterIP service)

Pod2 communicates with Host-Service (simulates an external host/service)

Local machine can access Pod1 or Host-Service using NodePort or port-forward

⚠️ Note: This setup is not production-ready but is ideal for learning and testing.


Why This Approach Was Chosen
Feature / Setup	Reasoning
Separate YAML files for each pod/service	Easier to understand and modify, minimal overhead for learners.
Host-Service Pod + NodePort	Simulates host/external service communication without relying on host IP.
NodePort + port-forward	Provides simple access from your local machine for testing/debugging.
ClusterIP for Pod1 → Pod2	Correct approach for pod-to-pod communication inside the Kubernetes cluster.
Avoid hardcoding host IPs	Ensures portability across machines and environments.

The focus is on simplicity, clarity, and hands-on testing over production best practices.

Architecture Overview
[External System / API / Host Service]
        ↑
        |  (DNS / NodePort / port-forward)
        ↓
[Kubernetes Service] <---> [Pod]

Pod1 and Pod2 communicate via a ClusterIP service.

Host-Service simulates external communication using a NodePort service.

The local machine can access services using NodePort or kubectl port-forward.

Getting Started

Apply all YAML files:

kubectl apply -f pod1.yaml
kubectl apply -f pod1-service.yaml
kubectl apply -f pod2.yaml
kubectl apply -f host-pod.yaml
kubectl apply -f host-service-svc.yaml

Check pod status:

kubectl get pods

Test communication:

From Pod2 to Pod1:

kubectl exec -it pod2 -- curl http://pod1-service:80

From Pod2 to Host-Service:

kubectl exec -it pod2 -- curl http://host-service:8080

From local machine:

curl <NodeIP>:<NodePort>