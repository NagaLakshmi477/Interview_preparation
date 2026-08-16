# Docker architecture
"Docker mainly has the Docker Client, Docker Host, and Docker Registry.
The Docker Client is the interface where we run commands such as docker build, docker run, and docker pull. The Docker Client communicates with the Docker daemon running on the Docker Host.

The Docker Host is the machine where the Docker daemon runs and where Docker containers are created and executed.

The Docker Registry is used to store and distribute Docker images. It can be a public registry such as Docker Hub or a private registry such as Amazon ECR."

"For example, if I run docker run nginx, Docker first checks whether the required Nginx image is available locally. If the image is not available locally, Docker pulls it from the configured registry, such as Docker Hub. Once the image is available, Docker creates and starts a container from that image."

"Docker also provides networking and volumes. Docker networks allow containers to communicate with each other, while volumes provide persistent storage for container data
