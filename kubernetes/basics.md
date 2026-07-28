## what is the diff b/w k8 and docker

- Docker is a container tool, and Kubernetes is a container orchestration tool.
- Kubernetes offers auto-scaling, load balancing, networking across multiple services and manages containers across multiple systems.
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
