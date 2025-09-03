# Cloning
Heroku Clone

Project Plan - Ideas, technologies

# Project Name

A platform designed to abstract and manage infrastructure for user projects through Docker, Kubernetes, and intelligent code analysis.  
The system aims to automatically determine resource requirements, optimize scaling, and provide resilience for workloads.

---

## Table of Contents
- [System Architecture](#system-architecture)
- [Core Technologies](#core-technologies)
- [Scaling & Resilience](#scaling--resilience)
- [CI/CD](#cicd)
- [Storage](#storage)
- [Roadmap](#roadmap)

---

## System Architecture

The base machines will run **Docker** and **Kubernetes (K8s)**.  
These will be responsible for abstracting and managing the infrastructure on which users’ projects will be executed.  

### Workflow

1. **Code Analyzer**  
   - Reads the user’s project to determine required resources (CPU, GPU, storage).  
   - Analyzes code dependencies and its nature.  
   - Builds the hardware abstraction layer.  

2. **Scaling**  
   Scaling is managed through a combination of components:  
   - **Analyzer:** Determines resource requirements and generates the allocation plan.  
   - **Metrics:** Collects performance data (CPU load, memory usage, etc.) to feed the scaling system.  
   - **Load Balancer:** Manages incoming traffic at both layer 4 (TCP/UDP) and layer 7 (HTTP), distributing the load across containers.  

---

## Core Technologies

### Code Analyzer
- **SonarQube** or **Cloud Native Buildpacks**: Analyze code and determine required resources.  
- Integrated with **Terraform** to create the necessary infrastructure.  
- A **Python script** will read analyzer metrics and configure Terraform accordingly.  
- Alternative option: provide a fixed baseline and delegate scalability to other components.  

### Containerization
- **Docker & Docker Compose**: For managing containers in development.  
- **Kubernetes (K8s)**: To orchestrate containers in production.  
- **Helm**: For managing configurations and deployments on Kubernetes.  

### Metrics
- **Prometheus**: Collects metrics related to performance, resource usage, and container health.  
- Provides input for the autoscaling system and monitoring.  

### Autoscaling
- **KEDA (Kubernetes Event-driven Autoscaling):** Manages autoscaling based on events and metrics from Prometheus.  
- **Redis:**  
  - Handles workload management and caching (TTL policy).  
  - Supports autoscaling for high-intensity workloads.  
  - Duplicates all incoming data on a **PersistentVolumeClaim (PVC)**.  
  - PVC garbage policies remove cache files every 24 hours, ensuring no data loss in crash scenarios.  

### Load Balancing
- **NGINX Ingress** or **Traefik**:  
  - Load balancer for both L4 (TCP/UDP) and L7 (HTTP).  
  - Distributes requests among Kubernetes pods and manages traffic balancing.  

### Security & Networking
- **Istio**:  
  - Manages service-to-service traffic.  
  - Provides security (TLS, authentication, authorization).  
  - Implements advanced routing policies for both ingress and internal cluster traffic.  

### CI/CD
- **Helm with Jenkins or GitLab**:  
  - Manages continuous integration and continuous delivery (CI/CD).  
  - Ensures code is tested, built, and deployed automatically.  

---

## Storage

- **Persistent Storage:**  
  Kubernetes PersistentVolumeClaims (PVC) for long-term data persistence.  

- **Non-persistent / Ephemeral Storage:**  
  - **Redis** or **Memcached** for caching and temporary data.  

---

## Scaling & Resilience

The system is designed with flexibility in resilience strategies.  
Two approaches are under consideration:  

1. **User-driven choice:** Users decide the replication policy.  
2. **Automated strategy:** Implement hybrid replication models:  
   - Start with **asynchronous replication**, then perform **synchronous copies** at intervals.  
   - Or adopt **semi-synchronous replication** from the beginning.  

---

## Roadmap

- [ ] Provide fixed infrastructure baseline with Kubernetes and Docker.  
- [ ] Integrate SonarQube / Buildpacks for code analysis.  
- [ ] Implement Python script to configure Terraform.  
- [ ] Add Redis caching + PVC garbage policies.  
- [ ] Enable Prometheus metrics collection.  
- [ ] Deploy KEDA autoscaling.  
- [ ] Add NGINX / Traefik ingress load balancing.  
- [ ] Implement Istio service mesh with security policies.  
- [ ] Configure CI/CD pipeline (Jenkins or GitLab with Helm).  
