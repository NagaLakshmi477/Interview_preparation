# 🚀 Docker Interview Notes 

---

# 📦 Docker Basics

## 1. Why do we need Docker?

Docker is needed because it solves a common problem: "it works on my machine but not on yours."
It is a tool that helps you run applications without worrying about setup.

---

## 2. Difference between Docker image and container Image

### Docker image:

* Docker image is a template for creating containers
* Built using a Dockerfile
* **Immutable**
* Stored in Docker registries (e.g., Docker Hub)
* Can be versioned and reused

### Docker Container:

* Running instance of a Docker image
* **Mutable (can change during runtime)**
* Multiple containers can be created from one image

“Immutable means the image cannot be changed after it is created; any update requires building a new image"

---

## 3. Can we modify an image?

Direct answer: ❌ No
But you can create a new image from a modified container using docker commit

---

# ⚙️ Dockerfile & Instructions

## 4. What is the difference between RUN, CMD, and ENTRYPOINT?

### Run:

* Executes commands during image build
* Used for installing packages and setup

### CMD:

* Defines the default command for container
* Runs when container starts
* Can be easily overridden

### Entrypoint:

* ENTRYPOINT sets a fixed command that always runs in the container
* Not easily overridden
* Runs every time container starts

---

## 5. What is the difference between ENV and ARG?

### ARG (Build-time Variable):

* Used to define variables during image build
* Available only at build time
* Not accessible after the image is created (not in container)
* Passed using --build-arg

### ENV (Runtime Variable):

* Used to define environment variables
* Available during build and runtime
* Accessible inside the running container
* Can be overridden at runtime (docker run -e)

---

## 6. What is LABEL used for?

* LABEL is used to add metadata to a Docker image
* metadata (author, version, description, etc.)
* It stores information as key-value pairs
* Helps in identifying, organizing, and managing images

---

## 7. What is a .dockerignore file?

A .dockerignore file tells Docker which files or folders to skip when building an image. It helps reduce image size and improves build performance.

---

## 8. How to optimize Docker layers?

* Combine multiple RUN commands using &&
* Use multi-stage builds to reduce final image size
* Order Dockerfile instructions properly (least changing → most changing)
* use lightweight images
* use .dockerignore file

---

## 9. Dockerfile Layer Caching Deep Dive?

Each instruction in a Dockerfile creates a new immutable layer
Docker reuses cached layers to speed up builds

### Best Practice Order

```
FROM:
    Base image
WORKDIR:
    Set working directory
COPY requirements.txt / package.json
    Copy dependency files first
RUN pip install / npm ci
    Install dependencies
    Cached unless dependency file changes
COPY . .
    Copy application code
    Changes frequently
```

---

# 🧠 Docker Internals

## 10. How Docker Works Internally (Beginner-Friendly)

Docker uses Linux operating system to create **containers**.
These containers look like separate machines, but they actually share the same OS kernel.

### Namespaces (Isolation)

* Processes
* Files
* Network

### Cgroups (Resource Control)

* CPU
* Memory
* Disk

### UnionFS / OverlayFS (Layered Filesystem)

* Base OS
* Software dependencies
* Application code

---

# ⚡ Docker Engine

## 11. What is a Docker Engine?

Docker Engine is the core software that runs and manages Docker containers. It acts as the "brain" of Docker.

### What Docker Engine Does

* Builds Docker images
* Runs containers
* Manages containers, networking, and storage
* **Consists of Docker daemon, CLI, and REST API**

---

# 📊 Monitoring & Security

## 12. Docker Metrics & Monitoring (Production Reality)

* docker stats → CPU, memory, network

### Production tools:

* cAdvisor
* Prometheus
* Grafana

### Flow:

* Containers generate metrics
* cAdvisor collects data
* Prometheus stores it
* Grafana displays it

---

## 13. Docker Secrets

Docker Secrets are used to securely store and manage sensitive information like passwords, API keys, and tokens without hardcoding them into images

---

## 14. Why should containers not run as root?

- By default, containers run as the **root** user.
- If an attacker gains access to the container, they may get **root privileges** inside the container.
- This increases the security risk and can affect the host if additional privileges are granted.


---

## 15. What is a dangling image?

A dangling image is a Docker image that has no tag and is not referenced by any container.
It usually gets created during image rebuilds or updates, when a new image replaces the old one but the old image loses its tag.These images are basically unused and take up disk space.”
- Remove using docker image prune

Remove using:

```
docker image prune
```

---

# 🌐 Networking

## 16. How to debug container networking?

Commands:

* docker ps
* docker network ls
* docker network inspect <network-name>
* docker inspect <container>
* docker exec -it <container> sh

Inside:

* ping <other-container>
* curl http://<service>:<port>

Advanced:

```
docker run -it --network <network_name> nicolaka/netshoot
```

---

## 17. How to check container IP address?

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <container_name_or_id>

- docker inspect --->  It shows everything in JSON format.
                        IP address
                        Network configuration
                        Mounted volumes
                        Environment variables
                        Container ID, paths, etc.
-f --> don't show everything show what i specify
NetworkSettings.Networks --> This is the field inside the josn.It contains all networks connected to the container
range --> Loops through all networks attached to the container.
end --> end the loop
```

---

## 18. Two containers (frontend & backend) need to communicate. How will you connect them?

* Create a user-defined bridge network and attach both containers to that network

```
docker network create roboshop

docker run -d --name backend --network roboshop mybackend

docker run -d --name frontend --network roboshop myfrontend
```

Containers can communicate using container names instead of IP addresses.

---

# 💾 Storage

## 19. Difference between volume and bind mount

### Volume:

* Managed by Docker
* Stored in Docker’s internal directory
* More secure
* Preferred in production

### Bind mount:

* Managed by host system(system used os)
* Uses existing host directory path
* Not portable
* Less secure

---

## 20. You need to share data between containers (like MySQL + app). How?

* A named volume is a storage area managed by Docker that exists outside the container
* It is used to persist data, so even if a container is deleted or recreated, the data remains safe

---

# 🚀 CI/CD

## 21. How do you integrate Docker in CI/CD pipelines?

### Steps:

* Build the image
* Push to registry
* Deploy the container

```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: docker build -t myapp:${{ github.sha }} .
      - run: docker push myapp:${{ github.sha }}
```

---

# 🧩 Real Interview Scenarios

## 22. What happens when you run docker run nginx?

* Docker checks if the image exists locally. If not, it pulls it from Docker Hub
* Then, it creates a new container from that image
* It sets up networking and assigns an IP
* Finally, it starts the nginx server inside the container
* Runs in foreground by default

---

## 23. You built an image but your latest code changes are not reflected. Why?

- Docker uses cached layers during image build. If no changes are detected in a layer, Docker reuses the previous layer, so  updated code may not be copied into the image.
- Rebuild the image without cache: docker build --no-cache -t your-image .
- change the order of COPY / RUN instructions in the Dockerfile to force cache invalidation.

```
docker build --no-cache -t your-image .
```
# 24. You deployed a container, but the application is not reachable from the browser. How would you troubleshoot it?

## Step 1: Check if the container is running

```bash
docker ps
```

- Verify that the container is in the **Up** state.
- If the container is not running, check why it exited.

---

## Step 2: Check container logs

```bash
docker logs <container_name>
```

- Look for application startup failures, exceptions, or configuration errors.

---

## Step 3: Verify the application is listening inside the container

```bash
docker exec -it <container_name> netstat -tuln
```

or

```bash
docker exec -it <container_name> ss -tuln
```

- Ensure the application is listening on the expected port.
- It should ideally listen on **0.0.0.0**, not **127.0.0.1**.

---

## Step 4: Verify port mapping

```bash
docker ps
```

Example:

```
0.0.0.0:8080->80/tcp
```

- Confirm that the container port is correctly mapped to a host port.

---

## Step 5: Test connectivity inside the container

```bash
docker exec -it <container_name> curl localhost:<port>
```

- If the application responds, it is running correctly inside the container.

---

## Step 6: Inspect Docker networking

```bash
docker inspect <container_name>
```

- Verify that the container has an IP address.
- Ensure it is connected to the correct Docker network.

---

## Step 7: Verify the Dockerfile

Check that the application is configured to use the correct port.

Example:

```dockerfile
EXPOSE 80
```

> **Note:** `EXPOSE` is only documentation. The port must still be published using the `-p` option when running the container.

---

## Step 8: Check firewall or cloud security rules

- Verify the host firewall allows the required port.
- If running in AWS, Azure, or GCP, ensure the Security Group/NSG/Firewall allows inbound traffic.

---

# Interview Answer (1 Minute)

> "First, I'd verify that the container is running using `docker ps`. Next, I'd check the container logs using `docker logs` to identify any application startup errors. Then I'd verify the application is listening on the expected port inside the container using `netstat` or `ss`. After that, I'd confirm the Docker port mapping with `docker ps` and test the application from inside the container using `curl localhost:<port>`. If the application works internally but is still inaccessible externally, I'd inspect the Docker network configuration and finally check the host firewall or cloud security group rules that might be blocking access."

---

## Troubleshooting Flow

```
Browser
    │
    ▼
Host Port Open?
    │
    ▼
Docker Port Mapping (-p)
    │
    ▼
Container Running?
    │
    ▼
Application Listening?
    │
    ▼
Application Logs
    │
    ▼
Docker Network
    │
    ▼
Firewall / Security Groups
```



## 26. You need to debug a container that immediately exits after starting.

- If a container immediately exits after starting, it usually means the main process (CMD or ENTRYPOINT) inside the container has finished or crashed.
- Since Docker containers run only while their main process is running, the container stops as soon as that process exits.
Check the container logs --> docker logs <container_name>
Check the exit code ---> docker inspect <container_name> | grep -i ExitCode
0 means it exited normally, while a non-zero value indicates an error
Start the container with an interactive shell --> docker run -it --entrypoint bash myimage

---

## 27. How do you handle multiple environments (Dev/Test/Prod)?

In my Immigration project, we followed the Build Once, Deploy Anywhere principle. We built the Docker image only once and used the same image in Dev, Test, and Production. Since each environment had different database credentials and configuration, we externalized them using environment variables. For example, DB_HOST, DB_USER, and DB_PASSWORD were different for each environment. This allowed us to keep the image unchanged while changing only the configuration, making deployments consistent and reducing environment-specific issues.

---

## 28. How to handle secret management in Docker?

- Instead of hardcoding passwords, API keys, or tokens inside Docker images
- we use Docker Secrets or secret management tools like HashiCorp Vault and AWS Secrets Manager.
- so sensitive data is not stored inside the image and is securely injected at runtime

---

## 29. Can you explain how Docker helps in microservices?

- Each microservice runs in its own container
- Has its own dependencies, libraries, and runtime
- No conflicts between services
- Same container runs in dev, test, and production
- Each microservice can be built, deployed, and updated separately
- Containers are lightweight and quick to start

---

## 30. You see high CPU usage inside a container. What do you do?

* Use docker stats
* Check logs
* Check running processes inside container
* Limit CPU using --cpus

---

# 31. COPY vs ADD

## COPY
- It will copy files and directories from local machine to Docker image  
- Docker will copy the file without changing anything  means  Same file → same content → same format  
- Local → Image → Container  
- Works only with local files  

---

## ADD
- COPY + extra features  
- It can also:
  1. Download files from URL  
  2. Automatically extract (untar) compressed files  

---

## Main Difference

| Feature | COPY | ADD |
|--------|------|-----|
| Local file copy | Yes | Yes |
| Download from URL | No | Yes |
| Auto extract tar | No | Yes |
| Recommended | Yes | Less preferred |

## 32. What does the EXPOSE instruction do?

- It is used to specify which port the container will use  
- It is only for documentation purpose  
- It does NOT actually open the port
## 33. What is USER used for?

- It is used to specify which user will run the container  
- By default, container runs as root user  
- USER instruction allows you to change to a non-root user

## 34. What is WORKDIR used for?
- It is used to set the working directory inside the container

## 35. What is ONBUILD? 

- It is used to add a trigger instruction in Docker image  
- It will not run while building the image  
- It will run when someone uses this image as a base image
## 36. What is Physical Server ?

**Architecture:**

Hardware (CPU, RAM, Motherboard) → Operating System → Application

**Notes:**

* Hardware includes CPU, RAM, and motherboard.
* The operating system (OS) is installed directly on the hardware.

---

## 37. what is Virtual Machines (VMs)

**Architecture:**

Hardware → Host OS → Hypervisor → Guest OS → Applications

**Notes:**

* **Hypervisor** is software that creates and manages virtual machines.
* VMs divide physical hardware into multiple virtual servers.
* Each VM runs its own **Guest OS** (Ubuntu, Windows, RedHat, CentOS, etc.).
* Requires installation and management of a hypervisor.
* Each VM has a separate OS, so it consumes more CPU, RAM, and storage.
* Resource usage is higher compared to containers.

---

## 38. What is  Containers / Docker

**Architecture:**

Hardware → Host OS → Docker Engine → Containers

**Notes:**

* Containers include only what is required to run the application.
* A container/image contains:

  * Minimal OS (base image)
  * System packages
  * Application code and dependencies
* **Docker Engine** is used to create and manage containers.
* Containers are lightweight and use less storage and memory.
* Containers share the Host OS kernel.
* No separate Guest OS is required.
* Resources are used dynamically and efficiently.

---

# VM vs Containers

| Feature             | Virtual Machines (VMs) | Containers           |
| ------------------- | ---------------------- | -------------------- |
| Cost                | High                   | Low                  |
| Startup Time        | More time              | Less time            |
| Size                | Large                  | Small                |
| Resource Usage      | More                   | Less                 |
| Resource Allocation | Fixed / Blocked        | Dynamic              |
| Extra Components    | Requires Hypervisor    | No extra components  |
| Portability         | Less portable          | Highly portable      |
| Security            | More secure            | Slightly less secure |


# Docker Images & Containers Commands

## Docker Images

### List all images
```bash
docker images
```
Displays all Docker images available on your local system.

> **Note:** A Docker image is a **read-only template** used to create containers.

### Pull an image with a specific tag
```bash
docker pull <image_name>:<tag>
```

**Example:**
```bash
docker pull nginx:latest
```

### Pull the latest version of an image
```bash
docker pull <image_name>
```

**Example:**
```bash
docker pull nginx
```

---

## Docker Containers

### Create a container (without starting it)
```bash
docker create <image_name>
```

**Example:**
```bash
docker create nginx
```

---

### List all containers (running and stopped)
```bash
docker ps -a
```

---

### List only running containers
```bash
docker ps
```

---

### Start a stopped container
```bash
docker start <container_id>
```

---

### Remove a container forcefully
```bash
docker rm -f <container_id>
```

---

### Remove an image
```bash
docker rmi <image_name>
```

**Example:**
```bash
docker rmi nginx
```

---

### Create and start a container
```bash
docker run <image_name>:<tag>
```

**Example:**
```bash
docker run nginx:latest
```

---

### Run a container in detached mode
```bash
docker run -d <image_name>
```

**Example:**
```bash
docker run -d nginx
```

---

### Run a container and map ports
```bash
docker run -d -p <host_port>:<container_port> <image_name>
```

**Example:**
```bash
docker run -d -p 80:80 nginx
```

---

### Run a container with a custom name
```bash
docker run -d -p 80:80 --name nginx nginx
```

---

### List only container IDs
```bash
docker ps -a -q
```

---

### Remove all containers
```bash
docker rm -f $(docker ps -a -q)
```

---

### Access a running container
```bash
docker exec -it <container_name> bash
```

**Example:**
```bash
docker exec -it nginx bash
```

---

### View the Nginx configuration file
```bash
cat /etc/nginx/nginx.conf
```

---

### Inspect a container
```bash
docker inspect <container_name>
```

**Example:**
```bash
docker inspect nginx
```

---

### View container logs
```bash
docker logs <container_name>
```

**Example:**
```bash
docker logs nginx
```

---

## Summary

| Command | Description |
|----------|-------------|
| `docker images` | List all images |
| `docker pull` | Download an image |
| `docker create` | Create a container without starting it |
| `docker run` | Create and start a container |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers |
| `docker start` | Start a stopped container |
| `docker exec -it` | Access a running container |
| `docker logs` | View container logs |
| `docker inspect` | View detailed container information |
| `docker rm -f` | Remove a container |
| `docker rmi` | Remove an image |


# Benefits of Docker Compose (Interview Points)

- Simplifies **multi-container application** management using a single `compose.yaml` file.
- Provides **consistent deployments** across Development, Testing, and Production environments.
- Automatically creates and manages **networking** between containers.
- Enables **service-to-service communication** using service names instead of IP addresses.
- Supports **service dependency management**, ensuring services start in the required order.
- Simplifies **application lifecycle management** (start, stop, rebuild, restart, and monitor services).
- Reduces manual effort by managing the entire application stack with a single command.
- Improves collaboration by storing the complete infrastructure configuration in a version-controlled Compose file.
- Accelerates local development and testing by creating identical environments for all developers.
- Makes application deployment simple, repeatable, and reliable.

# What is Image Caching in Docker?

Docker uses **layer caching** to speed up the image build process.

- Every instruction in a Dockerfile creates a separate image layer.
- When we build the image again, Docker checks whether a layer has changed.
- If a layer has **not changed**, Docker reuses the existing cached layer instead of rebuilding it.
- If a layer **changes**, Docker rebuilds that layer and all the layers after it.
- This reduces build time and improves efficiency.

# Why do we need container orchestration tools like Kubernetes?

Docker alone cannot handle **auto-scaling, self-healing, load balancing, service discovery, or rolling updates** for containerized applications.

Kubernetes automates the deployment, scaling, load balancing, health monitoring, rolling updates, and management of containers across a cluster of servers.
# how to integrate docker into jenkins

You
 │
 │ git push
 ▼
GitHub
 │
 │ Webhook / Poll SCM
 ▼
Jenkins
 │
 ├── Downloads the latest code
 ├── Builds the Docker image
 ├── Pushes the image to Docker Hub
 ├── Runs docker compose up -d
 └── Deploys the updated application

 ### How did you integrate Docker with Jenkins in your project?

In my project, we stored the application source code, Dockerfiles, and `compose.yaml` in a GitHub repository.

Initially, we built Docker images, pushed them to Docker Hub, and deployed the application using Docker Compose manually. To automate this process, we integrated Jenkins.

We created a **Declarative Jenkins Pipeline** with the following stages:

1. **Checkout** – Jenkins checks out the latest source code from the GitHub repository.
2. **Build** – It builds Docker images for the required services using their respective Dockerfiles.
3. **Docker Hub Login** – Jenkins securely logs in to Docker Hub using credentials stored in the Jenkins Credentials Manager.
4. **Push** – The newly built Docker images are pushed to Docker Hub.
5. **Deploy** – Jenkins executes `docker compose up -d` to deploy or recreate the updated containers.

Initially, we tested the pipeline manually using **Build Now**. After verifying that it worked successfully, we configured a **GitHub Webhook** so that every code push automatically triggered the Jenkins pipeline.

This eliminated the need to manually execute Docker commands such as `docker build`, `docker push`, and `docker compose up -d`. The entire build and deployment process became automated, ensuring faster and more consistent deployments.

# What is `docker commit` used for?
> `docker commit` is used to create a new Docker image from the current state of a running container. It is useful for quickly saving changes made inside a container during development or troubleshooting. However, in production, we generally avoid `docker commit` and use a Dockerfile to rebuild images because Dockerfiles provide a reproducible, version-controlled, and automated way to create images.


 # What happens when you stop and restart a container?

When a container is stopped, the container process is terminated, but the container itself is not deleted.

When the container is started again, Docker restarts the same container with the same filesystem, configuration, and container ID.

## Commands

Stop a container:

```bash
docker stop <container_name>
```

Start the same container again:

```bash
docker start <container_name>
```

## What is preserved?

- Container ID remains the same.
- Container IP address is usually preserved if the container is restarted on the same network.
- Files created inside the container are still available.
- Environment variables and container configuration remain unchanged.

## What is not preserved?

- Running processes are stopped and restarted.
- If the container was removed using `docker rm`, all container data is lost unless it is stored in a Docker volume or bind mount.

## Interview Answer

> When we stop a container, only the running process is terminated; the container is not deleted. When we start it again, Docker restarts the same container with its existing filesystem, configuration, and environment variables. However, if the container is removed, its data is lost unless it is stored in a Docker volume or bind mount.
