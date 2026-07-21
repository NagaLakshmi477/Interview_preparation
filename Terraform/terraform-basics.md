# 🚀 DEVOPS INTERVIEW NOTES (LEVEL-WISE)

---

# 🟢 LEVEL 1 — BASICS

## Explain the concept of "Infrastructure as Code" (IaC) and provide examples of popular IaC tools.

- **Infrastructure as Code (IaC)** means we can develop and manage entire infrastructure using code instead of manual setup.  

- A popular tool is **Terraform**, developed by HashiCorp, which uses HCL language that is easy and human-readable.  

- Using Terraform, we can create infrastructure across multiple cloud providers like Amazon Web Services, Microsoft Azure, and Google Cloud Platform, ensuring automation, consistency, and version control.  

- Other tools include AWS CloudFormation and Ansible.

---

## What is Continuous Integration (CI) and Continuous Deployment (CD), and how do they contribute to the DevOps pipeline?

- Continuous Integration (CI) is the process of automatically building and testing code whenever changes are made.  

- Continuous Deployment (CD) is the process of automatically deploying code to production after successful testing.  

- CI/CD helps in faster delivery, early bug detection, and improved software quality.

---

## Explain the role of version control systems in a DevOps workflow, and name some commonly used version control tools.

- Version Control System (VCS) is used to track and manage changes in code, allowing multiple developers to collaborate efficiently.  

- It helps in maintaining code history, rollback to previous versions, and supports branching and merging in a DevOps workflow.  

- A popular tool is Git, and commonly used platforms include GitHub, GitLab, and Bitbucket.

---

## What are microservices, and how do they contribute to a scalable and maintainable architecture in a DevOps environment?

- Microservices is an architecture where an application is divided into small, independent services, each handling a specific functionality.  

- Tools like Docker are used to deploy these services as containers.  

- Microservices help in scalability because each service can be scaled independently based on requirement.  

- They improve maintainability by allowing developers to update or fix one service without affecting the entire application, making it faster and more efficient.

---

## What is Docker?

- Docker is a containerization platform used to build, package, and run applications  

- It allows applications to run inside containers  

- Containers include:
  - application code  
  - dependencies  
  - libraries  

## Key Benefits

- Consistency across environments (dev, test, prod)  
- Lightweight compared to virtual machines  
- Fast deployment and scaling  
- Isolation of applications  

## Key Components

- Docker Image – blueprint of the application  
- Docker Container – running instance of an image  
- Dockerfile – instructions to build an image  

---

# 🟡 LEVEL 2 — INTERMEDIATE

## How do containers differ from virtual machines, and what benefits do containers bring to a DevOps environment?

### Virtual Machines (VM):
- In VMs, we need a **guest OS** on top of the host OS to run applications.  
- Resources like CPU and memory are **fixed/allocated**, even if not fully used, which can increase cost.  
- VMs are **heavier, slower to start**, and harder to maintain.  

### Containers (Docker):
- Containers do **not require a guest OS**; they share the host OS kernel.  
- Each container is isolated with its own **processes, network, and storage**.  
- Containers are **lightweight, fast, and use resources efficiently** based on requirement.  
- If one container fails, it does **not affect others**.  

### Benefits in DevOps:
- Faster deployment and scaling  
- Better resource utilization  
- Consistent environments across development, testing, and production  
- Easy integration with CI/CD pipelines  

---

## Describe the purpose of a Dockerfile and provide examples of Dockerfile instructions.

- A Dockerfile is a file where we write instructions to build Docker images, which are then used to create containers.  

- It automates the image creation process, making deployments consistent and repeatable.  

- Common Dockerfile instructions include:
  - FROM – specifies the base image  
  - RUN – executes commands during build  
  - CMD – defines the default command to run the container  
  - ENTRYPOINT – sets the main container command  
  - COPY / ADD – copies files into the image  
  - WORKDIR – sets the working directory  
  - USER – specifies the user to run the container  

---

## What is the role of automation in DevOps, and how does it contribute to the efficiency of software development and deployment?

- Automation in DevOps means reducing manual effort by using tools and scripts to perform tasks like build, testing, and deployment.  

- It helps improve application speed and efficiency by making processes faster, consistent, and less error-prone.  

- Automation enables continuous integration and continuous deployment (CI/CD), leading to faster and more reliable software delivery.

---

## How do you handle configuration management in a DevOps environment, and what tools have you used for this purpose?

- Configuration management is the process of automating system setup tasks like installing dependencies, managing services, and creating users.  

- It ensures systems are consistent, repeatable, and easy to manage across environments.  

- I have used Ansible, which is an agentless tool that uses playbooks to automate configuration tasks.

---

## What is Terraform, and how does it differ from other infrastructure as code tools?

- Terraform is an Infrastructure as Code (IaC) tool developed by HashiCorp that uses HCL (HashiCorp Configuration Language) to define and provision infrastructure.  

- It allows us to create and manage resources across multiple cloud providers like AWS, Azure, and GCP.  

- Unlike cloud-specific tools like AWS CloudFormation, Terraform is cloud-agnostic and supports multi-cloud environments, making it more flexible and widely used.

---

# 🔵 LEVEL 3 — ADVANCED

## Explain the concept of "declarative syntax" in Terraform and how it contributes to infrastructure management.

- Declarative syntax means we define the desired state of infrastructure, not the step-by-step process to create it.  

- In Terraform, we just specify what resources we need, and Terraform automatically figures out how to create and manage them.  

- This approach simplifies infrastructure management, ensures consistency, and reduces manual effort and errors.

---

## What are the key components of a Terraform configuration file?

- Provider block – defines the cloud provider (like AWS, Azure, GCP) and its configuration.  

- Resource block – defines the infrastructure components like VM, network, storage, etc.  

- Variables – used to make configurations reusable and dynamic.  

- Output values – used to display important information after deployment.  

- Dependencies – Terraform automatically manages dependencies between resources using references.

---

## How does Terraform maintain state, and why is state management important in infrastructure as code?

- Terraform maintains a state file (terraform.tfstate) that stores the current state of infrastructure.  

- The state can be stored locally or remotely (for example, in an S3 bucket) for better collaboration.  

- State management is important because Terraform compares the desired state (configuration) with the actual state (real infrastructure).  

- Without the state file, Terraform cannot determine what needs to be created, updated, or deleted.

---

## Describe the process of initializing a Terraform project. What does the terraform init command do?

- To initialize a Terraform project, we use the `terraform init` command.  

- It creates a `.terraform` directory in the current folder and downloads the required provider plugins based on the configuration.  

- It also initializes the backend (for state storage) and prepares the working directory for further commands like plan and apply.

---

## What is a Terraform provider, and how does it facilitate interactions with different infrastructure platforms?

- A Terraform provider is used to define which cloud or platform (like AWS, Azure, or GCP) we are using in the project.  

- In the provider block, we configure details like region and credentials required to connect to that platform.  

- Providers act as a bridge between Terraform and the infrastructure platform, allowing Terraform to create, update, and manage resources.

---

## Explain the purpose of the terraform plan command. What information does it provide, and why is it valuable?

- The `terraform plan` command checks the `.tf` configuration files and compares the desired state with the current state (from the state file).  

- It shows what changes will be made, such as resources to be created, updated, or deleted.  

- It is valuable because it gives a preview of changes before applying them, helping to avoid errors and ensuring safe infrastructure updates.

---

## What is the difference between Terraform's "provisioners" and "remote-exec" provisioner?

- Provisioners in Terraform are used to execute scripts or commands after a resource is created.  

- There are mainly two types:
  - local-exec – runs commands on the local machine  
  - remote-exec – runs commands on the remote server (created resource)  

- The remote-exec provisioner is specifically used to connect to a remote resource (like a VM) and execute commands on it.

---

## How to manage sensitive information in Terraform securely

- Never hardcode sensitive data (API keys, passwords) in .tf files  

- Use secure storage services:
  - AWS SSM Parameter Store  
  - AWS Secrets Manager  
  - Azure Key Vault  
  - GCP Secret Manager  

- Use environment variables:
  - Example: TF_VAR_db_password  

- Use Terraform variables with sensitive = true:
  - This hides values in output  

- Use remote backends (like S3 with encryption) to store state securely  

- Restrict access using IAM roles and policies  

---

## What are Terraform Workspaces?

- Terraform workspaces allow you to manage multiple environments using the same configuration  

- Each workspace maintains its own separate state file  

- This helps isolate environments like:
  - dev
  - staging
  - production  

---

## What are Terraform Modules?

- Terraform modules are reusable and self-contained pieces of Terraform code  

- A module can contain:
  - resources
  - variables
  - outputs  

- The default/root module is the main configuration, and we can create custom modules  

---

## What are Terraform Remote Backends?

- Remote backends store the Terraform state file in a remote location instead of locally  

- Examples:
  - AWS S3  
  - Azure Storage Account  
  - Terraform Cloud  

---

## What is the role of a Dockerfile?

- A Dockerfile is a script that contains instructions to build a Docker image  

- It defines:
  - base image (FROM)  
  - application setup  
  - dependencies installation  
  - commands to run the application  

- Docker reads the Dockerfile step by step to create an image  

---

## What is Docker Compose?

- it is yaml file (docker-compose.yml)  
- we can create multiple services (app, db, etc)  
- we can define dependencies between services  
- no need to worry about which service runs first  

- we can run all containers using single command  
  docker-compose up -d  

- we can stop all using  
  docker-compose down  
