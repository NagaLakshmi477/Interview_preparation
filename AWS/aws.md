# 1. Explain the AWS architecture used in your project.

## Answer

In my project, developers push the application code to a **GitHub repository**. This triggers a **GitHub Actions** workflow, which performs the following steps:

- Checks out the source code.
- Builds the application.
- Creates a Docker image.
- Pushes the Docker image to **Amazon ECR (Elastic Container Registry)**.

Once the image is available in ECR, the **EC2 instances** pull the latest Docker image and run the application as Docker containers.

These EC2 instances are registered behind an **Application Load Balancer (ALB)**, which distributes incoming user requests across multiple application instances to ensure high availability and load balancing.

Users access the application through **Amazon Route 53**, which maps the domain name to the ALB.

The application securely communicates with an **Amazon RDS** database deployed in **private subnets**, ensuring that the database is not directly accessible from the internet.

Sensitive information such as **database credentials, API keys, and application configuration** is stored in **AWS Systems Manager Parameter Store**. The application retrieves these secrets securely using **IAM Roles**, eliminating the need to hardcode credentials.

Finally, **Amazon CloudWatch** is used for centralized logging, infrastructure monitoring, application metrics, and alarms, enabling proactive monitoring and troubleshooting.

---

## Architecture Workflow

```text
Developer
    │
    ▼
GitHub Repository
    │
    ▼
GitHub Actions Pipeline
    │
    ├── Checkout Code
    ├── Build Application
    ├── Build Docker Image
    └── Push Image to Amazon ECR
            │
            ▼
     EC2 Instances pull Docker Image
            │
            ▼
     Run Application in Docker Containers
            │
            ▼
 Application Load Balancer (Public Subnets)
            ▲
            │
       Route 53 DNS
            ▲
            │
          Users

EC2 ─────────────► Amazon RDS (Private Subnets)

EC2 ─────────────► AWS Systems Manager Parameter Store
        ▲
        │
     IAM Role

EC2 ─────────────► Amazon CloudWatch
                  (Logs, Metrics & Alarms)
```# 2. What is Amazon ECR?

- **Amazon Elastic Container Registry (ECR)** is a fully managed Docker image registry provided by AWS.
- It is used to securely store, manage, and version Docker images.
- In our project, GitHub Actions builds the Docker image and pushes it to Amazon ECR.
- During deployment, EC2 instances pull the latest Docker image from ECR and run the application as Docker containers.

## Workflow

```text
GitHub Actions
       │
       ▼
Build Docker Image
       │
       ▼
Push Image to Amazon ECR
       │
       ▼
EC2 pulls the latest Docker Image
       │
       ▼
Runs the Docker Container
```

### Interview Answer

> Amazon ECR is AWS's managed Docker image registry. In our project, GitHub Actions builds the Docker image and pushes it to ECR. During deployment, EC2 instances securely pull the latest image from ECR using IAM Roles and run the application as Docker containers.

---

# 3. Why did you use Amazon ECR instead of Docker Hub?

## Docker Hub

```text
GitHub Actions
      │
      ▼
Push Image to Docker Hub
      │
      ▼
EC2 logs in using Docker Hub Username & Password
      │
      ▼
docker pull shopping-app:v1
```

- Need to manage Docker Hub credentials.
- Credentials must be stored securely on the EC2 instance or in the deployment pipeline.
- Pull rate limits may apply on free plans.

---

## Amazon ECR

```text
GitHub Actions
      │
      ▼
Push Image to Amazon ECR
      │
      ▼
EC2 authenticates using IAM Role
      │
      ▼
docker pull <ecr-image>
```

- No Docker Hub username or password is required.
- EC2 authenticates automatically using its IAM Role.
- Better integration with AWS services.
- Supports image vulnerability scanning.
- Supports lifecycle policies for automatic image cleanup.

### Interview Answer

> We chose Amazon ECR because our entire infrastructure was hosted on AWS. ECR integrates seamlessly with AWS services, and EC2 instances can securely authenticate using IAM Roles instead of Docker Hub credentials. This improves security, simplifies deployments, and provides additional features like image vulnerability scanning and lifecycle policies.

# 4. What is a Load Balancer?

- A **Load Balancer** is a service that distributes incoming user requests across multiple **EC2 instances**.
- It ensures that no single EC2 instance is overloaded and improves **high availability**, **scalability**, and **fault tolerance**.
- Users never connect directly to the EC2 instances. Instead, all requests first go to the **Application Load Balancer (ALB)**.

## How it works

- Users send requests to the Application Load Balancer (ALB).
- The ALB receives all incoming requests.
- It checks which EC2 instances are healthy.
- It distributes the requests among the healthy EC2 instances.
- If an EC2 instance becomes unhealthy, the ALB automatically stops sending traffic to it.
- Traffic is routed only to the remaining healthy EC2 instances.

This ensures that the application remains available even if one of the EC2 instances fails.

# 5. What is Amazon CloudWatch?

- **Amazon CloudWatch** is a monitoring and logging service provided by AWS.
- It is used to monitor the health and performance of AWS resources and applications in real time.
- CloudWatch collects **logs, metrics, and events** from AWS services and applications.
- It also helps create **dashboards** and configure **alarms** to notify the team when a threshold is exceeded (for example, CPU utilization above 80%).

## Workflow

```text
EC2 Instance
     │
     ▼
Application Logs
System Logs
CPU Metrics
Memory Metrics
Network Metrics
     │
     ▼
Amazon CloudWatch
     │
     ├── Store Logs
     ├── Monitor Metrics
     ├── Create Dashboards
     └── Trigger Alarms (if threshold is crossed)
```

---

# What is the difference between CloudWatch Logs and CloudWatch Metrics?

### CloudWatch Metrics
- Numerical values that represent the performance of AWS resources.
- Examples:
  - CPU Utilization
  - Memory Utilization
  - Network Traffic
  - Disk I/O
  - Request Count

### CloudWatch Logs
- Log files generated by applications or operating systems.
- Used for troubleshooting and debugging.
- Examples:
  - Application errors
  - Exception messages
  - Access logs
  - System logs
# Why is ALB placed in the public subnet?
The Application Load Balancer is placed in a public subnet because it needs to receive incoming traffic from internet users. Route 53 points our domain to the ALB, which accepts the requests and forwards them to EC2 instances running in private subnets. This architecture improves security because the EC2 instances are not directly exposed to the internet. Only the ALB is internet-facing, while the application servers and the RDS database remain protected in private subnets.
# Can an ALB be placed in a private subnet?
Yes. If the ALB is used only for internal communication within the VPC, we place it in private subnets. If it needs to receive traffic from internet users, we place it in public subnets as an internet-facing ALB.
# Why are application servers in private subnets?
Users
   │
   ▼
Internet
   │
   ▼
Application Load Balancer
(Public Subnet)
   │
   ▼
EC2 Application Servers
(Private Subnet)
   │
   ▼
Amazon RDS
(Private Subnet)

# What is an Image Lifecycle Policy?

- An **Amazon ECR Image Lifecycle Policy** automatically deletes old or unused Docker images based on rules that we define.
- It helps reduce storage costs and keeps the ECR repository clean.
- Instead of manually deleting old images, ECR removes them automatically according to the lifecycle policy.

## Example

Suppose our CI/CD pipeline builds a new Docker image for every deployment.

```text
prod:v1
prod:v2
prod:v3
...
prod:v10
prod:v11
```

If we configure a lifecycle policy to **keep only the latest 10 images**, ECR automatically deletes **prod:v1** when **prod:v11** is pushed.

## Example Lifecycle Policy

```json
{
  "rules": [
    {
      "rulePriority": 1,
      "description": "Keep only last 10 images",
      "selection": {
        "tagStatus": "tagged",
        "tagPrefixList": ["prod"],
        "countType": "imageCountMoreThan",
        "countNumber": 10
      },
      "action": {
        "type": "expire"
      }
    }
  ]
}
```

## Interview Answer

> An Image Lifecycle Policy in Amazon ECR automatically deletes old or unused Docker images based on rules that we define. In our project, we configured a lifecycle policy to keep only the latest 10 production images. Whenever a new image was pushed, ECR automatically removed the oldest image. This helped reduce storage costs and kept the repository clean.

# What happens if the deployment fails?

- If the deployment fails, the new version of the application is not deployed successfully.
- If the failure occurs before replacing the existing application, users continue accessing the previous version.
- If the existing application has already been stopped and the new version fails to start, the application becomes temporarily unavailable.
- We troubleshoot the issue, fix it, and redeploy the application or roll back to the previous stable version if required.

---

# How do you troubleshoot a deployment failure?

- First, I check the **GitHub Actions workflow logs** to identify at which stage the deployment failed.
- If the issue is on the EC2 instance, I verify whether the Docker container is running using:
  ```bash
  docker ps -a
  ```
- If the container has stopped or exited, I inspect its logs using:
  ```bash
  docker logs <container-id>
  ```
- I also check the application logs to identify the root cause.
- Once I identify the issue, I fix it and redeploy the application.

# How do you roll back?
Rollback means reverting the application from the current failed version to the previous stable version.
# Where are pipeline secrets stored?
- the pipeline secrets were stored in GitHub Actions Secrets.
  GitHub Repository
       │
       ▼
GitHub Actions
       │
       ▼
Reads GitHub Secrets
       │
       ▼
Authenticates to AWS
       │
       ▼
Pushes Docker Image to Amazon ECR

# IAM User vs IAM Role

| IAM User | IAM Role |
|----------|----------|
| A permanent identity created for a person or an application. | A temporary identity that provides permissions to AWS services or trusted identities. |
| Used by users such as DevOps Engineers, Developers, or Administrators. | Used by AWS services such as EC2, Lambda, ECS, and EKS. |
| Uses long-term credentials (Username/Password, Access Key ID, Secret Access Key). | Uses temporary credentials automatically provided by AWS. |
| Credentials must be created, stored, and rotated manually. | No credentials need to be stored or managed manually. |
| Can sign in to the AWS Management Console or use AWS CLI/API. | Cannot sign in to the AWS Management Console directly. It is assumed by an AWS service or user. |
| Best suited for human users. | Best suited for AWS services and applications. |
| Example: A DevOps Engineer logging in to AWS. | Example: An EC2 instance pulling Docker images from Amazon ECR or reading secrets from Parameter Store. |

---

## Interview One-Liner

**IAM User:** Used for people who need long-term access to AWS.

**IAM Role:** Used for AWS services to securely access other AWS resources using temporary credentials without storing access keys.

# Why use SSM Parameter Store or Secrets Manager?

- **AWS Systems Manager Parameter Store** and **AWS Secrets Manager** are AWS services used to securely store and manage configuration values and sensitive information.
- They help avoid hardcoding values such as passwords, API keys, and configuration details in application code or Terraform files.

## How we used Parameter Store in our project:

- We used AWS Systems Manager Parameter Store to store and retrieve Terraform outputs such as:
  - VPC IDs
  - Subnet IDs
  - Security Group IDs
  - ALB details

- Different Terraform modules can retrieve these values from Parameter Store instead of passing them manually between modules.

Example:

```text
VPC Module
     │
     ▼
Creates VPC
     │
     ▼
Stores VPC ID in Parameter Store
     │
     ▼
Other Terraform Modules
     │
     ▼
Retrieve VPC ID from Parameter Store
```

---

# AWS Secrets Manager

- AWS Secrets Manager is used to store sensitive information such as:
  - Database passwords
  - API keys
  - Application credentials

- It provides additional features like:
  - Automatic secret rotation.
  - Better integration with databases.
  - Managing frequently changing credentials.

---

# Why choose Parameter Store instead of Secrets Manager?

> We chose Parameter Store because our use case was mainly storing and retrieving infrastructure configuration values such as VPC IDs, subnet IDs, and security group IDs. It provided secure storage, IAM integration, and was a cost-effective solution. If we needed automatic secret rotation for database credentials or frequently changing secrets, we would use AWS Secrets Manager.
# What goes to S3
- we use S3 for storing Terraform remote state files, application build artifacts like JAR and WAR files, logs such as CloudTrail and ALB access logs, and backup data.
-  S3 provides highly durable storage, and we enable features like encryption, versioning, and lifecycle policies for security and cost optimization
# what is ECS and Fargate?
- ECS is AWS's container orchestration service. It helps us run Docker containers
- Fargate is a serverless compute option that runs ECS
 - **ECR** = Where we keep the Docker image  
- **ECS** = Who manages and runs the containers  
- **Fargate** = Where the containers run without managing servers

# what is SNS & SQS ?
 **SNS (Simple Notification Service)**
- SNS is a messaging service used for **sending notifications or messages to multiple subscribers**.
- It follows a **publish/subscribe (Pub/Sub) model**.
- When a publisher sends a message to an SNS topic, SNS pushes that message to all subscribed services.
 **SQS (Simple Queue Service)**
- SQS is a message queue service used for **storing messages temporarily and processing them asynchronously**.
- It helps decouple different components of an application.
- The message remains in the queue until a consumer processes it.
