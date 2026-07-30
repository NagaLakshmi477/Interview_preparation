## what are the some challenges with promotheus

despite of very good at k8s promotheus still have some issues



challanges
==============
## resource sharing
- How you can allocates resources onmultiple envi(dev,prod,QA)
As a devops engineer when joining on a organization there was a cluster that shared across all environments
beacuse of one of pod leaking memory entire cluster was impacted so we don't know which pod is going down and which namespaces
beacuse of out of memory as a devops engineer i ahve setup the resouce quota and resource limit on the pods of the particular namespace
beacuse of which i can identify which pod is 
or
As a DevOps engineer, when I joined an organization, there was a Kubernetes cluster shared across multiple environments like Dev, QA, and Prod.

One day, one of the Pods had a memory leak and started consuming more memory. Because there were no proper resource limits, it impacted the entire cluster, and it was difficult to identify which Pod or namespace was consuming more resources.

So, as a DevOps engineer, I set up ResourceQuota for each namespace and configured resource requests and limits for the Pods.

Because of this, we were able to control how much CPU and memory each namespace and Pod could consume. If any Pod started consuming excessive resources, it would not impact the entire cluster, and we could easily identify which Pod or namespace was causing the issue.

- OOM killed issue with POD
so after setting the resuorce quota you know that one pod is getting OOM killed which is a crashloopbackoff
so what i have was this is java based micro service so i login into this pod and shared head dump and tread dump and shared those to developer team
or
If a Pod is getting OOMKilled, first I identify which Pod is consuming more memory.

In my case, after setting the resource quotas and resource limits, I identified that one of the Pods was getting OOMKilled and going into CrashLoopBackOff.

This was a Java-based microservice. So, I checked the Pod and collected the Java heap dump and thread dump to understand why the application was consuming more memory.

I shared the heap dump and thread dump with the development team so they could analyze the application and identify the memory leak or memory-related issue.

Based on their analysis, we worked on fixing the issue and then monitored the Pod to make sure it was running properly without getting OOMKilled again.

- Upgrades

For Kubernetes upgrades, we prepared a very detailed manual with step-by-step procedures.

Before performing the upgrade, I documented how to take the required backups and how to check the Kubernetes release notes and upgrade requirements.

I divided the upgrade process into two main parts: the control plane components and the worker nodes.

For the control plane, I documented the order of the components to be upgraded and how to verify the health of important components like `etcd` and the API server before and after the upgrade.

For the worker nodes, before upgrading a node, I first drain the node so that the workloads are safely moved to other available nodes. Then I perform the upgrade and verify that the node comes back to the `Ready` state.

After upgrading all the nodes, I test the cluster and verify that all the Pods, Services, and applications are working properly.

We documented the complete process in a manual so that the upgrade could be performed in a controlled way and the same steps could be followed by other team members as well.


## k8 architecture

We have a control plane and worker nodes.

On the control plane, we have different components:

API Server
etcd
Scheduler
Controller Manager
Cloud Controller Manager

The API Server is a core component. It is present on the control plane and is used to expose Kubernetes.
This Kubernetes cluster has to be exposed to the external world.
It is like the heart of Kubernetes and takes requests from the external world.
The Scheduler is used to schedule components on nodes. It is responsible for scheduling resources or pods on Kubernetes.
etcd acts like a backup server. It is basically a key-value store where the entire Kubernetes cluster information is stored as key-value pairs.
The Controller Manager supports auto-scaling, for example: ReplicaSet.
data plain(worker nodes)
---------------------
There are 3 components:

Kubelet
Container Runtime
Kube Proxy

The request will go through the control plane.

We have a component called Kubelet. It is responsible for running your pods and maintaining them.
If a pod is not running, Kubernetes informs it to restart.

For running pods, we need a container runtime (like containerd).

Kube Proxy provides networking. It handles IP addresses and load balancing.

## Kubernetes Architecture (Interview Notes)

Kubernetes architecture is mainly divided into two parts:

1. Control Plane
2. Worker Nodes (Data Plane)

---

## Control Plane (Brain of Kubernetes)

The control plane manages the entire Kubernetes cluster.

**Key Components:**

* **API Server**
  The core component and entry point to Kubernetes.
  It exposes the Kubernetes cluster to external users and handles all incoming requests.

* **etcd**
  A key-value store that stores all cluster data and configuration.
  It acts as the source of truth for the cluster.

* **Scheduler**
  Responsible for assigning pods to worker nodes based on resource availability.

* **Controller Manager**
  Ensures the desired state of the cluster is maintained.
  Example: Handles ReplicaSets for auto-scaling and self-healing.

* **Cloud Controller Manager**
  Manages interactions with the cloud provider (if using cloud platforms).

---

## Worker Nodes (Data Plane)

Worker nodes are responsible for running applications (pods).

**Key Components:**

* **Kubelet**
  An agent that runs on each node.
  It ensures containers are running as expected and restarts them if needed.

* **Container Runtime**
  Used to run containers (e.g., containerd, Docker).

* **Kube Proxy**
  Handles networking inside the cluster.
  Manages IP routing and load balancing between services.

---

## Flow (How it works)

1. User sends a request → API Server
2. API Server validates and stores data in etcd
3. Scheduler assigns the pod to a worker node
4. Kubelet runs the pod using container runtime
5. Kube Proxy manages networking and access

---

## Key Points to Impress Interviewers

* API Server is the **entry point** of Kubernetes
* etcd is the **database (source of truth)**
* Scheduler decides **where pods run**
* Kubelet ensures **pods are always running**
* Kube Proxy handles **networking and load balancing**
* Kubernetes follows **desired state management**


## Deployement strategies

we have 3 types
1. Rolling update (by default)
2. blue green
3. canary

Uisng ingress controller we will configure by default this are k8 out of box
so why we need deplooyment means to reduce the download for doing upgradations and also we make zero down time
rolling update deployement stargy it is intrduce the down time to zero
using rolling update we can perform zero down time version upgrade we can reach to near downtime 
