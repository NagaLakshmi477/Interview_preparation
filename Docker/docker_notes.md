# Docker architecture
"Docker mainly has the Docker Client, Docker Host, and Docker Registry.
The Docker Client is the interface where we run commands such as docker build, docker run, and docker pull. The Docker Client communicates with the Docker daemon running on the Docker Host.

The Docker Host is the machine where the Docker daemon runs and where Docker containers are created and executed.

The Docker Registry is used to store and distribute Docker images. It can be a public registry such as Docker Hub or a private registry such as Amazon ECR."

"For example, if I run docker run nginx, Docker first checks whether the required Nginx image is available locally. If the image is not available locally, Docker pulls it from the configured registry, such as Docker Hub. Once the image is available, Docker creates and starts a container from that image."

"Docker also provides networking and volumes. Docker networks allow containers to communicate with each other, while volumes provide persistent storage for container data

# what is cluster
"Kubernetes has one control plane and multiple worker nodes. So, the control plane is responsible for managing and coordinating the cluster, and normally we run our application workloads on the worker nodes.

The control plane gives the instructions and the worker nodes execute the workloads. If one worker node crashes, Kubernetes detects the node failure and the Pods running on that node can be recreated on another healthy worker node, depending on the workload configuration
We can store our application data or backup data in Amazon S3. When we use eksctl to create an EKS cluster, it creates the EKS cluster in AWS. The EKS architecture has a managed control plane and one or more worker nodes. The control plane is managed by AWS, and our application workloads normally run on the worker nodes.

# Kubernetes namespaces
A namespace is like an isolated space inside a Kubernetes cluster where we can create and manage our resources. We can use different namespaces for different environments or teams, like dev, UAT, and production. It helps us separate resources and also apply access control and resource limits
# CrashLoopBackOff
CrashLoopBackOff can occur when a container starts and then exits or crashes repeatedly. For example, if we create a container using the Nginx image, Nginx has a foreground process running, so the container continues running.

But if we use a basic AlmaLinux image without giving it a long-running command or process, the container may start and then exit automatically. Kubernetes will try to restart the container again and again. Because the container keeps failing, Kubernetes eventually shows the CrashLoopBackOff status

# Container vs Pod
A Pod is the smallest deployable unit in Kubernetes, and a Pod can contain one or multiple containers. Normally, we run one main application container in one Pod, but in some cases we can have multiple containers in the same Pod, for example a sidecar container.
The containers inside the same Pod share the same network namespace, so they use the same IP address and can communicate with each other using localhost. They can also share storage volumes if we configure them.
# why 2 containers in pod

One purpose of having multiple containers in the same Pod is the sidecar pattern. For example, the main application container continuously generates logs and writes them to a shared location such as /var/log. The sidecar container reads those logs from the shared volume and forwards them to a logging system such as Elasticsearch. Elasticsearch stores and indexes the logs, and we can search and analyze them later

# Kubernetes Labels vs Annotations.
Labels are key-value pairs attached to Kubernetes resources. They are mainly used to identify and select resources. For example, Services use label selectors to identify the Pods they should send traffic to.

A label value normally has a limit of 63 characters. Labels are important for grouping and selecting resources.

Annotations are also key-value pairs, but they are not used as selectors. They are mainly used to store additional metadata or information for external tools and applications, such as URLs, build information, image details, or other configuration metadata."
# Resource Limiting:
In Kubernetes, we can define CPU and memory resources for each container based on its requirement. We can specify resource requests and limits. The request defines the amount of CPU and memory the container needs to run, and the limit defines the maximum resources the container can use.

For example, if one application needs more CPU and memory, we can specify higher requests and limits. For another lightweight application, we can specify lower values. This helps Kubernetes schedule Pods properly and prevent one container from consuming all the resources on a node.
# Horizontal vs Vertical Scaling
Vertical scaling means increasing the resources of the same server, such as CPU, memory, or storage. Depending on the infrastructure, increasing resources may require a restart or downtime. It also has a scaling limit because we are depending on a single server.

To avoid these limitations, we can use horizontal scaling, where we increase the number of Pods or servers. For example, if one Pod is getting more traffic, we can create additional Pods and distribute the traffic across them using a Kubernetes Service or load balancer.

Because we have multiple Pods, if one Pod fails, other healthy Pods can continue serving traffic. So horizontal scaling improves availability and can help us achieve zero or minimal downtime during scaling and failures.
# How i can create multiple pods to same image
Using a Deployment, we can create multiple Pods using the same container image. For example, if we specify three replicas, Kubernetes will maintain three Pods.

If one Pod is deleted or fails, the ReplicaSet automatically creates another Pod to maintain the desired number of replicas.

The relationship is: Deployment manages the ReplicaSet, and the ReplicaSet manages the Pods
