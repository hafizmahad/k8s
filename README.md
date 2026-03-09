Kubernetes Minimal Demo: Pod Communication & Host-Service
Overview
This repository demonstrates a minimal Kubernetes setup for learning purposes:

Pod1 and Pod2 communicate inside a cluster.
Host-Service simulates an external service or host, allowing pods to communicate with a "host" (local machine or another service).
Access the pods/services from your local machine using NodePort or port-forwarding.
All Kubernetes resources are defined in separate YAML files for modularity.


⚠️ This setup is for local development/testing only.


Repository Structure
textk8s-demo/
├── host-pod.yaml           # Pod simulating a host/external service
├── host-service-svc.yaml   # NodePort Service for host pod
├── pod1.yaml               # Frontend Pod
├── pod1-service.yaml       # NodePort Service for Pod1
├── pod2.yaml               # Backend Pod
├── README.md               # This file
└── .gitignore              # Ignore temp/log files

1️⃣ Purpose of This Repo
This repo demonstrates minimal Kubernetes pod communication for learning and local development:

Pod2 communicates with Pod1 (ClusterIP service)
Pod2 communicates with Host-Service (simulates host/external service)
Windows host can access Pod1 or Host-Service using NodePort or port-forward


⚠️ Note: This setup is simplified for local development and testing.
It is not production-ready, but it avoids complex configs and overhead.


2️⃣ Why This Approach Was Chosen
Feature / SetupReasoningSeparate YAML files for each pod/serviceEasier to understand and modify, minimal overhead for learners.Host-Service Pod + NodePortSimulates host/external service communication without relying on host IP.NodePort + port-forwardProvides simple access from Windows host for testing/debugging.ClusterIP for Pod1 → Pod2Correct way for pod-to-pod communication inside Kubernetes cluster.Avoid hardcoding host IPsEnsures portability across machines and environments.

Essentially: We prioritize simplicity, clarity, and hands-on testing over production best practices.


3️⃣ Architecture Diagram
text[External System / API / Host Service]
        ↑
        |  (DNS / NodePort / port-forward)
        ↓
[Kubernetes Service] <---> [Pod]