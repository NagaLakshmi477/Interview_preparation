# Kubernetes & Prometheus — Interview Notes

## 1. Challenges with Prometheus

Prometheus is very good for Kubernetes monitoring, but in large environments there can still be challenges related to **resource management, OOM issues, scalability, and upgrades**.

---

### 1.1 Resource Sharing Across Multiple Environments

One challenge is sharing Kubernetes resources across multiple environments such as:

* Development
* QA
* Production

#### Real-World Scenario

As a DevOps Engineer, when I joined an organization, there was a Kubernetes cluster shared across multiple environments such as Dev, QA, and Production.

One day, one of the Pods had a memory leak and started consuming excessive memory. Because there were no proper resource limits, it impacted the entire cluster, and it was difficult to identify which Pod or namespace was consuming the resources.

#### Solution

As a DevOps Engineer, I configured:

* `ResourceQuota` at the namespace level
* `Resource Requests`
* `Resource Limits` for Pods/containers

For example:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: namespace-quota
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
```

I also configured resource requests and limits for application containers.

This helped us:

* Control CPU and memory consumption
* Prevent one namespace from consuming unlimited resources
* Identify which namespace was consuming excessive resources
* Reduce the impact of resource exhaustion on other workloads

### Important distinction

**ResourceQuota** controls the total resource consumption allowed for a namespace.

**Resource requests/limits** control the resources requested/allowed for individual containers.

---

# 2. OOMKilled Issue with Pod

Another common Kubernetes challenge is when a Pod gets terminated with an:

```text
OOMKilled
```

This means the container exceeded its available memory limit or the node experienced memory pressure.

The Pod may subsequently enter:

```text
CrashLoopBackOff
```

## Real-World Scenario

After configuring resource requests, limits, and quotas, I identified that one of the Pods was repeatedly getting:

```text
OOMKilled
```

and eventually going into:

```text
CrashLoopBackOff
```

The application was a Java-based microservice.

## Troubleshooting

First, I checked the Pod:

```bash
kubectl get pod <pod-name>
```

Then I checked the details:

```bash
kubectl describe pod <pod-name>
```

I also checked the previous container logs:

```bash
kubectl logs <pod-name> --previous
```

Since it was a Java application, I worked with the development team to collect diagnostic information such as:

* Java heap dump
* Thread dump

The heap dump helped the developers investigate excessive heap usage and potential memory leaks.

I shared the diagnostic information with the development team so they could analyze the application and identify the root cause.

After the application-side issue was addressed, we monitored the Pod to make sure it remained stable and did not get OOMKilled again.

### Important Interview Point

Do not simply say:

> "I increased the memory limit."

A better approach is:

> "I first identify why the application is consuming excessive memory. Increasing the limit may temporarily hide the problem but does not necessarily fix the root cause."

---

# 3. Kubernetes Upgrade Challenges

Kubernetes upgrades can be challenging because the cluster contains multiple components and workloads that need to remain compatible and available.

## Real-World Approach

For Kubernetes upgrades, we prepared a detailed manual with step-by-step procedures.

Before performing the upgrade, we documented:

* Backup requirements
* Kubernetes version compatibility
* Release notes
* Upgrade prerequisites
* Application compatibility checks
* Rollback/recovery procedures

The upgrade process was divided into:

1. Control plane
2. Worker nodes

## Control Plane

Before upgrading the control plane, we verified the health of important components such as:

* API Server
* etcd
* Scheduler
* Controller Manager

We also ensured that the required etcd backup/recovery procedures were available.

After the upgrade, we verified that the control plane components were healthy.

## Worker Nodes

Before upgrading a worker node, we safely moved workloads away from that node.

For example:

```bash
kubectl drain <node-name> --ignore-daemonsets
```

Then the node was upgraded.

After the upgrade, we verified:

```bash
kubectl get nodes
```

and ensured that the node returned to:

```text
Ready
```

After all worker nodes were upgraded, we performed application-level validation.

We checked:

* Pods
* Services
* Deployments
* Application connectivity
* Application health

The complete process was documented so that the upgrade could be performed consistently and safely by other team members.

---

# 4. Kubernetes Architecture

Kubernetes architecture is mainly divided into two parts:

1. **Control Plane**
2. **Worker Nodes / Data Plane**

```text
                 Kubernetes Cluster
                        |
          +-------------+-------------+
          |                           |
    Control Plane                 Worker Nodes
       (Brain)                    (Data Plane)
          |                           |
    +-----+------+              +-----+------+
    |     |      |              |     |      |
 API   Scheduler etcd        Kubelet Runtime Kube-Proxy
Server
    |
Controller
Manager
```

---

# 5. Control Plane Components

The Control Plane manages the overall Kubernetes cluster and maintains the desired state.

## 5.1 API Server

The API Server is the main entry point to the Kubernetes API.

It:

* Receives requests from users, automation, and Kubernetes components
* Validates requests
* Authenticates/authorizes requests
* Exposes the Kubernetes API
* Communicates with etcd
* Acts as the central communication point between Kubernetes components

Example:

```text
kubectl
   |
   ↓
API Server
```

### Interview Statement

> "The API Server is the central entry point to the Kubernetes API. It receives and processes requests and acts as the communication hub for Kubernetes components."

---

# 6. etcd

`etcd` is a distributed key-value store used by Kubernetes to store cluster state.

It stores information such as:

* Cluster configuration
* Kubernetes objects
* Desired state
* Metadata
* Secrets and configuration data

It is the **source of truth for Kubernetes cluster state**.

### Important Correction

Do not describe etcd as simply:

> "A backup server."

That is incorrect.

Better:

> "etcd is the distributed key-value store and source of truth for Kubernetes cluster state."

Backups of etcd can be taken, but **etcd itself is not a backup server**.

---

# 7. Scheduler

The Kubernetes Scheduler is responsible for deciding **which worker node should run a newly created Pod**.

It considers factors such as:

* CPU and memory requests
* Node availability
* Node selectors
* Affinity/anti-affinity
* Taints and tolerations
* Other scheduling constraints

Example:

```text
New Pod
   |
   ↓
Scheduler
   |
   +----→ Worker Node 1
   |
   +----→ Worker Node 2
   |
   +----→ Worker Node 3
```

### Interview Statement

> "The Scheduler is responsible for selecting an appropriate worker node for a Pod based on resource availability and scheduling constraints."

---

# 8. Controller Manager

The Controller Manager runs various Kubernetes controllers.

Controllers continuously compare:

```text
Desired State
      vs
Current State
```

and take action to bring the cluster back to the desired state.

Examples include:

* ReplicaSet controller
* Deployment-related controllers
* Node controller
* Job controller

For example, if a Deployment requires 3 Pods and only 2 are running, the controllers work to create the missing Pod.

### Important Correction

Don't say:

> "Controller Manager supports auto-scaling."

Auto-scaling is handled by dedicated mechanisms such as **Horizontal Pod Autoscaler (HPA)**.

Better:

> "The Controller Manager runs controllers that continuously reconcile the desired state with the current state."

---

# 9. Cloud Controller Manager

The Cloud Controller Manager integrates Kubernetes with cloud-provider infrastructure.

For example, it can manage cloud-specific resources such as:

* Load balancers
* Nodes
* Routes
* Cloud-specific integrations

The exact behavior depends on the cloud provider and Kubernetes setup.

---

# 10. Worker Nodes / Data Plane

Worker nodes are responsible for running application workloads.

The major components are:

1. Kubelet
2. Container Runtime
3. kube-proxy

---

# 11. Kubelet

Kubelet is the Kubernetes agent running on each worker node.

It:

* Communicates with the API Server
* Ensures containers described by Pods are running
* Monitors Pod/container status
* Reports node and Pod status back to Kubernetes

Kubelet works with the container runtime to start and manage containers.

### Important Correction

Instead of saying:

> "Kubelet runs the Pods."

A more technically accurate statement is:

> "Kubelet ensures that the containers specified by Pods are running on the node by communicating with the container runtime."

---

# 12. Container Runtime

The container runtime is responsible for running containers.

Examples include:

* containerd
* CRI-O

The Kubernetes kubelet communicates with the container runtime through the Container Runtime Interface (CRI).

```text
Kubelet
   |
   ↓
Container Runtime
   |
   ↓
Containers
```

---

# 13. kube-proxy

`kube-proxy` is a node-level networking component traditionally responsible for implementing Kubernetes Service networking rules.

It helps route traffic destined for Services toward their backend Pods.

### Important Correction

Don't say:

> "kube-proxy handles all Kubernetes networking."

Kubernetes networking is broader than kube-proxy and also involves the cluster's networking implementation/CNI.

Better:

> "kube-proxy implements Service networking rules on nodes and helps route Service traffic to backend Pods."

---

# 14. Kubernetes Request Flow

A simplified flow when creating a Deployment/Pod is:

```text
User
 |
 | kubectl apply
 ↓
API Server
 |
 ↓
etcd
 |
 ↓
Scheduler
 |
 ↓
Worker Node
 |
 ↓
Kubelet
 |
 ↓
Container Runtime
 |
 ↓
Container / Pod
```

The controllers continuously monitor the cluster and reconcile the desired state.

---

# 15. Kubernetes Deployment Strategies

Common deployment strategies include:

1. Rolling Update
2. Blue-Green Deployment
3. Canary Deployment

---

# 16. Rolling Update

Rolling Update is the **default Deployment strategy** for Kubernetes Deployments.

Instead of stopping all old Pods and then starting new Pods, Kubernetes gradually replaces old Pods with new Pods.

Example:

```text
Version 1

Pod V1
Pod V1
Pod V1

       ↓ Rolling Update

Pod V1
Pod V1
Pod V2

       ↓

Pod V1
Pod V2
Pod V2

       ↓

Pod V2
Pod V2
Pod V2
```

### Advantages

* Gradual rollout
* Reduced disruption
* Easy rollback
* No need to manually stop the entire application

The goal can be **zero downtime**, provided the application, readiness probes, capacity, and rollout configuration are designed appropriately.

### Important Correction

Don't say:

> "Rolling update always gives zero downtime."

Better:

> "Rolling Update can provide zero or near-zero downtime when the application and Kubernetes configuration are designed correctly."

---

# 17. Blue-Green Deployment

In Blue-Green deployment, two versions of the application are maintained:

```text
Blue  → Current Version
Green → New Version
```

Example:

```text
             Service
                |
                ↓
           Blue Version
           Version 1
```

After testing the new version:

```text
             Service
                |
                ↓
          Green Version
          Version 2
```

Traffic is switched from Blue to Green.

### Advantages

* Easy rollback
* New version can be tested before switching traffic
* Fast traffic switch

### Disadvantage

It may require additional infrastructure/resources because both versions may need to run simultaneously.

---

# 18. Canary Deployment

In a Canary deployment, only a small percentage of traffic is initially sent to the new version.

Example:

```text
              Users
                |
                ↓
          Traffic Router
           /          \
          /            \
       90%              10%
        ↓                 ↓
    Version 1          Version 2
```

If Version 2 is healthy, traffic can gradually increase:

```text
90% / 10%
   ↓
70% / 30%
   ↓
50% / 50%
   ↓
0% / 100%
```

Canary deployments are useful for reducing deployment risk.

---

# 19. Ingress and Deployment Strategies

Ingress is **not itself a deployment strategy**.

Ingress is primarily used to manage HTTP/HTTPS traffic entering the cluster.

Depending on the Ingress controller and configuration, traffic routing can be used to support patterns such as:

* Blue-Green
* Canary
* Host-based routing
* Path-based routing

However, these deployment strategies are not automatically provided simply because Ingress exists.

---

# 20. Why Do We Need Deployment Strategies?

Deployment strategies help us:

* Reduce application downtime
* Reduce deployment risk
* Perform controlled version upgrades
* Gradually release new versions
* Roll back when problems occur
* Improve application availability

---

# 21. Quick Interview Revision

### Prometheus/Kubernetes Challenges

```text
Resource Sharing
       ↓
ResourceQuota
       +
Resource Requests/Limits

OOMKilled
       ↓
Check Pod
       ↓
Check logs
       ↓
Check previous logs
       ↓
Heap/Thread dump for Java
       ↓
Developer analysis
       ↓
Fix root cause

Kubernetes Upgrade
       ↓
Backup
       ↓
Check compatibility/release notes
       ↓
Upgrade control plane
       ↓
Drain worker node
       ↓
Upgrade worker
       ↓
Uncordon/verify
       ↓
Application validation
```

### Kubernetes Architecture

```text
CONTROL PLANE
│
├── API Server
├── etcd
├── Scheduler
├── Controller Manager
└── Cloud Controller Manager

WORKER NODE
│
├── Kubelet
├── Container Runtime
└── kube-proxy
```

### Deployment Strategies

```text
Rolling Update → Gradually replace old Pods
Blue-Green     → Switch traffic between two versions
Canary         → Gradually expose users to new version
```

### Key Interview Corrections

| Avoid saying                                | Better statement                                                               |
| ------------------------------------------- | ------------------------------------------------------------------------------ |
| etcd is a backup server                     | etcd is the cluster's key-value store/source of truth                          |
| Controller Manager does auto-scaling        | Controllers reconcile desired vs current state; HPA handles Pod autoscaling    |
| Kubelet runs Pods                           | Kubelet ensures Pod containers are running via the container runtime           |
| kube-proxy handles all networking           | kube-proxy implements Service networking rules                                 |
| Every Rolling Update gives zero downtime    | Rolling Update can provide zero/near-zero downtime when properly configured    |
| Ingress is a deployment strategy            | Ingress manages incoming HTTP/HTTPS traffic and can support routing patterns   |
| ResourceQuota limits an individual Pod      | ResourceQuota limits aggregate namespace resource usage                        |
| Pod OOMKilled always means CrashLoopBackOff | OOMKilled can cause restarts; repeated failures can result in CrashLoopBackOff |
