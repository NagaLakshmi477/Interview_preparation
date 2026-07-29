## what is the diff b/w k8 and docker

Docker is a containerization tool used to build, package, and run applications as containers. Kubernetes is a container orchestration platform that manages containers at scale. For example, if I have multiple microservices running as containers across multiple servers, Kubernetes can manage them by providing features like auto-scaling, load balancing, networking, and self-healing. So, Docker helps me run containers, while Kubernetes helps me manage containers at scale
## what are the main componnets in k8 architecture


## what are the diff b/w docker swarm and kubernetes

## what is the diff b/w docker conatiner and kubernetes pod
- pod is the smallest deployment unit in kubernetes
- a pod in kubernetes is used to run one or more containers
- a pod contain one or more containers
- containers inside the pod use the same IP and same networking
- docker container is the running instance of docker image
- container run on the host machine
- pod can run on the nodes

## what is the namespace in kubernetes
- namespace is an isolated space where we can create our workloads
- we can create resources like pods, services, deployments inside a namespace
- mainly used for managing multiple environments like dev, test, prod
- policies, rbac and networking rules can be applied per namespace (no overlap between namespaces)

## what is the role of kubeproxy


## what are the differnt types of services within kubernetes

1. cluster IP
- ClusterIP is the default Service type.
- If we don't mention any Service type, Kubernetes creates ClusterIP by default.
- ClusterIP is used for internal communication.
- It is not exposed directly to the outside world.
Pod → Service (ClusterIP) → Pod

2. nodePort
- NodePort is used to expose the application outside the Kubernetes cluster.
- It will open one port called node port in every node
- External users can access the application using the Node IP + NodePort.

Example:

External User
      ↓
Node IP + NodePort
      ↓
Service
      ↓
Pod

3. Load blancer
- LoadBalancer is also used to expose the application outside the Kubernetes cluster.
- In a cloud environment like AWS, it creates a cloud Load Balancer.
- The Load Balancer sends traffic to the application through the Kubernetes Service.

External User
↓
Load Balancer (External IP)
↓
Service
↓
Pod

## what is the role of kubelet


## day day to activites on k8


## What is a Kubernetes Node?

- A Kubernetes cluster consists of one or more nodes.
- A node is a physical or virtual machine that runs containerized applications.
- If one node fails, Kubernetes automatically shifts the workloads to another healthy node.
- Each node has three main components:
  1. **Kubelet** – Communicates with the Kubernetes control plane and manages Pods on the node.
  2. **Container Runtime** – Runs containers, such as `containerd`.
  3. **Kube-proxy** – Handles network communication and routing for Kubernetes Services.

## What is Deployment in Kubernetes?

- A Deployment is a Kubernetes resource that manages the lifecycle of Pods.
- With the help of a Deployment, we can define the desired state and provide high availability.
- If any problems occur, we can easily roll back to a previous version.
- Example: If we create 3 replicas, Kubernetes makes sure that 3 Pods are up and running.

## How does kubernates performing load blancing
- Kubernetes provides internal load balancing using Services.
- A Service provides a stable IP address and DNS name for a group of Pods and distributes traffic among them.
- Example: If we deploy a web application with 3 replicas, the Kubernetes Service distributes incoming traffic across the 3 Pods.

## A pod keeps crashing. how do you troubleshoot
If a Pod keeps crashing, I first check the Pod status using `kubectl get pods` and then check the specific Pod using `kubectl get pod <pod-name>`.

Next, I check the container logs using `kubectl logs <pod-name>`. If the container has restarted, I use `kubectl logs <pod-name> --previous` to check the logs from the previous container instance.

Then, I check the Pod events using `kubectl describe pod <pod-name>`. This helps me identify issues such as `OOMKilled`, failed health probes, image pull errors, or volume mount problems.

I also check the resource requests and limits and verify the node where the Pod is running.

Based on the error and events, I identify the root cause, fix the issue, and verify that the Pod is running successfully and is no longer restarting.



## what is configMap in k8
or 
## What if your database host changes after you've already built your Docker image? Do you rebuild the image again? No! That's where Kubernetes ConfigMap comes in
- A ConfigMap is a Kubernetes API object used to store non-sensitive configuration data separately from the container image.
ex: For example, instead of hardcoding application configuration such as the database host, application environment I can store it in a ConfigMap and mount it as a file.
This allows us to change the configuration without rebuilding the container image.


## A service is not reaching the correct pods? how do you debug
or
## Your Kubernetes Service is running, your Pods are running—but traffic is still not reaching the right Pod. What went wrong?

If a Kubernetes Service is not reaching the correct Pods, I first check the Service using `kubectl get svc` and `kubectl describe svc catalogue.

Then I verify the Service selector and compare it with the labels on the Pods using `kubectl get pods --show-labels`. The Service selector must match the Pod labels; otherwise, the Service will not select the correct Pods.

I also check the Pod status and readiness because a Pod that is not ready may not receive traffic.

Based on what I find, I fix the selector, Pod labels, or readiness issue and then test the Service again.
ex
NAME          LABELS
catalogue-1   app=catalogue
catalogue-2   app=catalogue
catalogue-3   app=user
## what is the purpose of namespace in k8
It allows multiple applictaions use the same cluster without any conflict. They logically separate the resources 
For example, in a real-time environment, we can have separate namespaces for dev, test, and prod. This allows multiple teams or applications to use the same cluster while keeping their resources logically separated.

🚨 **"What if your Dev and Production applications are running in the same Kubernetes cluster? How do you keep them organized?"**

That's where **Kubernetes Namespaces** come in!

Imagine I have one Kubernetes cluster with two environments: `dev` and `prod`.

I can create two separate namespaces:

`dev`

and

`prod`

Now I can run my `catalogue` application in both namespaces without conflicts.

You can also apply different RBAC permissions and resource quotas to different namespaces.

🔥 **In simple words: A namespace logically separates and organizes resources inside a Kubernetes cluster.**

For example: **Dev, Test, and Prod can have their own namespaces while using the same cluster.**


## how do you scale a k8 deployement
kubectl scale deployment <deployement-name> --replicas=5
🚨 **"Your application suddenly gets 10 times more traffic. Your 2 Pods can't handle the load. What do you do?"**

Imagine my Roboshop `catalogue` application is running with 2 Pods.

Suddenly, traffic increases.

I can manually scale the Deployment from 2 Pods to 5 Pods using:

`kubectl scale deployment catalogue --replicas=5`

Now Kubernetes creates additional Pods until I have 5 replicas running.

I can verify them using:

`kubectl get pods`

But remember, this is **manual scaling**.

If I want Kubernetes to automatically increase or decrease the number of Pods based on resource utilization, I can use **HPA — Horizontal Pod Autoscaler**.

🔥 **Manual scaling: `kubectl scale`**

🔥 **Automatic scaling: HPA**

## how does k8 handle rolling updates and roll backs?
or
## You deployed a new version to production, and suddenly your application starts failing. Do you panic and redeploy everything? No! Kubernetes can roll back to the previous stable version.
Kubernetes Deployments handle rolling updates by gradually replacing the old version of the application with the new version instead of stopping all Pods at once. This helps minimize downtime and keeps the application available during the deployment.

For example, if my Roboshop catalogue application is running version 1 and I deploy version 2, Kubernetes gradually creates new Pods with version 2 and removes the old version 1 Pods.

If the new version has an issue, I can check the rollout status and history using:

`kubectl rollout status deployment/<deployment-name>`

`kubectl rollout history deployment/<deployment-name>`

If I need to revert to the previous stable version, I can use:

`kubectl rollout undo deployment/<deployment-name>`

This allows me to quickly roll back the application to a previous revision.


## what is a deamon set
or
## Want to run one Pod on every Kubernetes node automatically? Don't create Pods manually—use a DaemonSet!
A DaemonSet is a Kubernetes controller that makes sure a Pod runs on every node in the cluster.

For example, if I have 3 nodes and I want to collect logs from all the nodes, I can use a DaemonSet. Kubernetes will automatically run one log collector Pod on each node.

If a new node is added, Kubernetes automatically creates the same Pod on the new node.

So, in simple words, **DaemonSet means: run one Pod on every node.**
