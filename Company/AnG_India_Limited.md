

# ANG – DevOps Interview Questions & Answers

---

## 1. Your Terraform pipeline is failing because Terraform State is locked. What would you do?

If my Terraform pipeline fails because the state is locked, first I would **not immediately force-unlock it**. I would check whether another Terraform operation or Jenkins pipeline is currently running against the same state.

I would check the lock information, including the **lock ID, user, operation, and timestamp**. If another Terraform operation is running, I would wait for it to complete.

If I confirm that the lock is stale because a previous pipeline failed or was terminated unexpectedly, I would use:

```bash
terraform force-unlock <LOCK_ID>
```

to remove the stale lock.

After unlocking, I would run:

```bash
terraform plan
```

to verify that the state and infrastructure are consistent, and then rerun the pipeline.

The important point is that I would use `force-unlock` **only after confirming that no other Terraform operation is running**.

### Flow

```text

   Terraform Pipeline
       ↓
State Locked?
       ↓
      YES
       ↓
Check Lock Information
       ↓
Is another Terraform operation running?
       ↓
   ┌─── YES ───→ WAIT
   │               ↓
   │        Operation finishes
   │               ↓
   │          Lock released
   │
   └─── NO ───→ Lock is probably stale
                    ↓
          Confirm previous operation
          crashed/was terminated
                    ↓
       terraform force-unlock <LOCK_ID>
                    ↓
             terraform plan
                    ↓
             Check the plan
                    ↓
             terraform apply
```

---

## 2. How do you manage secrets in Terraform?

I don't hardcode secrets directly in Terraform code. I prefer storing secrets in a secure service such as **AWS Secrets Manager or SSM Parameter Store** and retrieving them when required.

I also mark Terraform variables containing sensitive information as:

```hcl
variable "db_password" {
  type      = string
  sensitive = true
}
```

This prevents the value from being displayed in normal Terraform CLI output.

Since Terraform can store sensitive values in the **state file**, I secure the remote state as well. For AWS, I use an **S3 backend with encryption and proper IAM permissions**, and restrict access to the state file.

I also make sure that `.tfvars` files containing secrets are not committed to Git.

For example, in `.gitignore`:

```gitignore
*.tfvars
*.tfstate
*.tfstate.*
```

### Important point

`sensitive = true` **does not encrypt the secret in the Terraform state file**. It mainly prevents the value from being displayed in CLI output.

Therefore, protecting the Terraform state is very important.

---

## 3. Where do you save those secrets?

I save secrets in **AWS Secrets Manager or SSM Parameter Store**. I don't hardcode them in Terraform files or commit them to Git.

For example, database passwords and API keys can be stored in Secrets Manager, and Terraform or the application can retrieve them when required.

I also make sure access is controlled using **IAM permissions**.

### Example

```text
Application / Terraform
          ↓
AWS Secrets Manager
          ↓
Database Password / API Key
```

Only the required IAM role or user should have permission to access the secret.

---

## 5. How do you retrieve RDS database passwords and things like that?

For RDS credentials, I don't hardcode the database password in Terraform. I store the credentials in **AWS Secrets Manager**.

In Terraform, I can use the **Secrets Manager data source** to retrieve the secret and use the username and password while creating or managing the RDS instance.

I also secure the Terraform remote state because sensitive values can be stored in the state file. I use encryption and proper IAM permissions to restrict access to the state.

### Example

```hcl
data "aws_secretsmanager_secret" "db" {
  name = "prod/rds"
}

data "aws_secretsmanager_secret_version" "db" {
  secret_id = data.aws_secretsmanager_secret.db.id
}
```

If the secret contains JSON such as:

```json
{
  "username": "admin",
  "password": "mypassword"
}
```

Terraform can decode the secret and use the required values.

### Interview point

I would also mention that **reading a secret through Terraform can cause the secret value to be present in Terraform state**, depending on how it is used.

So I would secure the backend and restrict access to the state.

---

# 7. What is the difference between Terraform and AWS CloudFormation?

Terraform and AWS CloudFormation are both **Infrastructure as Code tools**, but the main difference is that Terraform is **cloud-agnostic**, while CloudFormation is an **AWS-native IaC service**.

Terraform can manage resources across multiple cloud providers such as AWS, Azure, and GCP using providers.

CloudFormation is mainly designed for managing AWS infrastructure and integrates deeply with AWS services.

Terraform uses **HCL** configuration files, while CloudFormation commonly uses **YAML or JSON**.

Another difference is state management. Terraform maintains a **state file** to track the relationship between the configuration and the infrastructure. CloudFormation manages the stack state through AWS.

### Example

With Terraform:

```text
Terraform Code
      ↓
Terraform Provider
      ↓
AWS API
      ↓
AWS Resources
```

With CloudFormation:

```text
CloudFormation Template
        ↓
AWS CloudFormation
        ↓
AWS Resources
```

### Simple comparison

| Terraform                        | CloudFormation          |
| -------------------------------- | ----------------------- |
| HashiCorp tool                   | AWS service             |
| Multi-cloud                      | AWS focused             |
| Uses HCL                         | Uses YAML/JSON          |
| Uses Terraform state             | AWS manages stack state |
| Uses providers                   | Native AWS integration  |
| Can manage AWS, Azure, GCP, etc. | Primarily AWS           |

### Interview answer

If the company is primarily using AWS and wants deep AWS-native integration, CloudFormation can be a good choice.

If the organization works with multiple cloud providers or wants a common IaC tool across environments, Terraform is often preferred.

---

# 8. What is Terraform Apply?

```text
Terraform Code (.tf)
       ↓
terraform plan
       ↓
Compare:
Desired State ↔ Current State
       ↓
Show Changes
       ↓
Ask for Confirmation
       ↓
terraform apply
       ↓
AWS API
       ↓
EC2 created/modified
       ↓
Terraform State updated
```

`terraform apply` is used to actually create, modify, or delete infrastructure according to the Terraform configuration.

When we run `terraform apply`, Terraform compares the desired configuration with the current state, creates an execution plan, and after approval it makes the required changes through the cloud provider APIs.

After the changes are completed, Terraform updates the state file.

For example, if I define an EC2 instance in my Terraform code and run:

```bash
terraform apply
```

Terraform will create that EC2 instance in AWS.

The main difference is that:

```bash
terraform plan
```

only shows what changes will happen, whereas:

```bash
terraform apply
```

actually performs those changes.

### Important interview point

In a CI/CD pipeline, we can also generate a plan file:

```bash
terraform plan -out=tfplan
```

and then apply that exact plan:

```bash
terraform apply tfplan
```

This helps ensure that the changes that were reviewed during the plan stage are the changes actually applied.

---

# 10. How do you maintain Terraform files for the Dev environment and Production environment?

```text
terraform/
│
├── modules/
│   ├── vpc/
│   ├── ec2/
│   ├── rds/
│   └── eks/
│
└── environments/
    ├── dev/
    │   ├── main.tf
    │   ├── variables.tf
    │   ├── terraform.tfvars
    │   └── backend.tf
    │
    └── prod/
        ├── main.tf
        ├── variables.tf
        ├── terraform.tfvars
        └── backend.tf
```

I maintain Dev and Production environments separately, but I avoid duplicating the Terraform code.

I create reusable Terraform modules for common infrastructure such as **VPC, EC2, RDS and EKS**. Then I have separate environment directories for Dev and Prod, where I provide environment-specific variables and configuration.

For example, Dev may use smaller instance types and fewer replicas, while Production uses larger instances and higher availability.

I also maintain separate Terraform state for each environment, usually using separate paths in an S3 backend.

This approach gives us **reusable code, environment isolation, and makes it easier to manage Dev and Production independently**.

### Flow

```text
                 Terraform Code
                       ↓
                Reusable Modules
                       ↓
             ┌─────────┴─────────┐
             ↓                   ↓
            DEV                 PROD
             ↓                   ↓
       Dev Variables       Prod Variables
             ↓                   ↓
        Dev Resources      Prod Resources
             ↓                   ↓
        dev.tfstate       prod.tfstate
```

### Important point

I would make sure that Dev and Prod do **not accidentally share the same Terraform state**.

Separate state helps prevent a change intended for Dev from affecting Production.

---

# 12. What is the difference between a Container and a VM?

A **Virtual Machine** virtualizes the hardware, while a **Container** virtualizes the application environment at the operating-system level.

A VM contains a complete guest operating system along with the application and required libraries.

A container shares the host operating system kernel and contains the application, libraries, and dependencies required to run it.

### VM

```text
Physical Server
       ↓
    Hypervisor
       ↓
 ┌─────────────┬─────────────┐
 ↓             ↓             ↓
 VM            VM            VM
 ↓             ↓             ↓
Guest OS      Guest OS      Guest OS
 ↓             ↓             ↓
App           App           App
```

### Container

```text
Physical Server
       ↓
 Host OS
       ↓
Container Runtime
       ↓
 ┌─────────────┬─────────────┐
 ↓             ↓             ↓
Container     Container     Container
 ↓             ↓             ↓
App           App           App
```

### Main differences

| VM                             | Container                               |
| ------------------------------ | --------------------------------------- |
| Includes complete guest OS     | Shares host OS kernel                   |
| Heavier                        | Lightweight                             |
| Takes more resources           | Uses fewer resources                    |
| Usually slower to start        | Starts quickly                          |
| Stronger OS-level isolation    | Process-level isolation                 |
| Suitable for full OS workloads | Suitable for microservices/applications |

### Simple interview answer

For example, if I run a microservice application, I can package the application and its dependencies into a Docker container. Containers are lightweight and start quickly, so they are very useful for microservices and CI/CD environments.

---

# 13. What is CMD and what is ENTRYPOINT in Docker?

Both `CMD` and `ENTRYPOINT` define what should run when a Docker container starts, but they behave differently.

### CMD

`CMD` provides the **default command or default arguments** for the container.

It can be overridden when we run the container.

Example:

```dockerfile
FROM ubuntu

CMD ["echo", "Hello"]
```

If I run:

```bash
docker run myimage
```

it executes:

```text
echo Hello
```

But I can override it:

```bash
docker run myimage echo Hi
```

Now it executes:

```text
echo Hi
```

---

### ENTRYPOINT

`ENTRYPOINT` defines the **main executable** of the container.

Example:

```dockerfile
FROM ubuntu

ENTRYPOINT ["echo"]
```

If I run:

```bash
docker run myimage Hello
```

the container executes:

```text
echo Hello
```

The argument `Hello` is passed to the `ENTRYPOINT`.

---

### CMD + ENTRYPOINT

They are commonly used together.

```dockerfile
FROM ubuntu

ENTRYPOINT ["echo"]
CMD ["Hello"]
```

Running:

```bash
docker run myimage
```

results in:

```text
echo Hello
```

If I run:

```bash
docker run myimage Hi
```

the `CMD` value is replaced, so it becomes:

```text
echo Hi
```

### Simple way to remember

```text
ENTRYPOINT = What the container runs

CMD = Default argument / default command
```

A common practical use is to use `ENTRYPOINT` for the application's main executable and `CMD` for default arguments that can be overridden.

---

# 14. What is SonarQube and what is the use of it?

## My SonarQube Implementation Journey

When I joined the project, we had no SonarQube integration in place. Code quality, coverage, and vulnerabilities were not being measured at all.

I was assigned the responsibility of onboarding all project components into SonarQube and ensuring developers adopted the tool effectively.

Since it was a major shift, I decided to roll this out in stages over **3–4 months**.

---

## Phase 1 – Initial Setup (Month 1)

I configured the **SonarQube server** and integrated it with Jenkins pipelines.

I added the SonarQube scanner plugin and configured the SonarQube server in Jenkins.

I created SonarQube projects and requested each team to add:

```text
sonar-project.properties
```

in the root of their repositories.

Then I ran the first scans to generate baseline reports for:

* Bugs
* Code smells
* Vulnerabilities
* Maintainability
* Code coverage
* Duplications

### Purpose

The purpose of the first phase was to give visibility to developers about the current state of their code **without immediately blocking the pipeline**.

---

# Phase 2 – Developer Adoption (Months 2–3)

I educated developers on how to interpret SonarQube reports.

I set expectations that they had approximately **2.5 months to fix issues**, including:

* Bugs
* Vulnerabilities
* Code smells
* Maintainability issues
* Coverage issues

I held weekly tracking sessions to monitor progress and help teams clear blockers.

I encouraged best practices such as:

* Writing unit tests
* Removing duplications
* Refactoring long methods
* Fixing security issues
* Improving maintainability

---

# Phase 3 – Enforcing Quality Gates (Month 3–4)

After developers became familiar with the reports, I introduced **SonarQube Quality Gates** in the CI pipeline.

Initially, I configured the pipeline to show warnings but **not fail the builds**.

I gave developers a **2-week buffer** to adapt to the gate requirements.

After 2 weeks, I enabled strict enforcement.

If the code failed the Quality Gate because of issues such as:

* Insufficient coverage
* Bugs
* Security issues
* Other configured quality conditions

the pipeline was aborted automatically.

---

# End-to-End SonarQube Flow

```text
Developer
    ↓
Git Repository
    ↓
Jenkins Pipeline
    ↓
Checkout Code
    ↓
Install Dependencies
    ↓
Run Unit Tests
    ↓
Generate Code Coverage Report
    ↓
SonarQube Scanner
    ↓
SonarQube Server
    ↓
Code Analysis
    ↓
Quality Gate
    ↓
 ┌───────────────┴───────────────┐
 ↓                               ↓
PASS                            FAIL
 ↓                               ↓
Continue Pipeline               Stop Pipeline
 ↓
Docker Build
 ↓
Docker Image
 ↓
Deployment
```

---

# Jenkins Integration

In Jenkins, we configured the SonarQube server and scanner.

The pipeline performs the SonarQube analysis after the code is tested.

A typical flow is:

```text
Checkout
   ↓
Install Dependencies
   ↓
Unit Tests
   ↓
Coverage Report
   ↓
SonarQube Scan
   ↓
waitForQualityGate()
   ↓
Quality Gate Result
   ↓
Continue / Abort
```

We also configured a **webhook from SonarQube to Jenkins** so Jenkins could receive the Quality Gate result.

For example:

```groovy
waitForQualityGate()
```

If the Quality Gate passed, Jenkins continued to the next stages such as Docker build and deployment.

If the Quality Gate failed, Jenkins aborted the pipeline.

---

# Outcome

Within 3–4 months, we achieved:

* Consistent code quality checks across all project components.
* Higher unit test coverage.
* Teams were motivated to reach at least **80% coverage**.
* Reduced vulnerabilities and better security posture.
* Improved maintainability of the codebase.
* Developers became self-reliant in using SonarQube reports to improve their code.

Because of the impact on code quality and team adoption, I was recognized with the **Best Performer Award** in the project.

### Short interview summary

If the interviewer asks for the complete flow, I would explain:

> "When I joined the project, SonarQube was not integrated. I onboarded the project components, integrated SonarQube with Jenkins, established baseline reports, worked with developers to fix the issues, and initially kept the Quality Gate non-blocking. Once the teams adapted, we enforced the Quality Gate using Jenkins and a SonarQube webhook. The pipeline would continue to Docker build and deployment only when the Quality Gate passed."

---

# 15. What is a code smell?

A code smell is an indication that the source code has a potential **maintainability, readability, or design problem**.

It doesn't necessarily mean the code is incorrect or that the application will fail.

For example, if a method contains hundreds of lines and is responsible for multiple tasks, SonarQube may report it as a code smell because it is difficult to understand, test, and maintain.

We can fix the smell by refactoring the code, such as breaking a large method into smaller methods with clear responsibilities.

In our SonarQube implementation, we used code-smell reports to help developers identify and improve maintainability issues in their code.

### Example

```text
Large Method
     ↓
Multiple Responsibilities
     ↓
Difficult to Read
     ↓
Difficult to Test
     ↓
Code Smell
     ↓
Refactor
     ↓
Smaller Methods
```

### Important point

A **code smell is not necessarily a bug**.

The application may work correctly, but the code may be difficult to maintain or extend.

---

# 17. What is code coverage?

Code coverage is a metric that tells us how much of our application code is executed by our automated tests.

For example, if we have 100 lines of executable code and our unit tests execute 80 lines, the code coverage is 80%.

In our project, we integrated SonarQube with Jenkins.

During the Jenkins pipeline, after checking out the code and installing dependencies, we ran the unit tests and generated the code coverage report.

The SonarQube Scanner then picked up the coverage report along with the source code analysis and sent the results to the SonarQube server.

SonarQube displayed coverage for both the overall codebase and new code.

For new code, SonarQube determined what is considered new based on the project's configured **New Code Definition**.

Initially, we enabled the Quality Gate without blocking the pipeline so developers could understand and fix the existing issues.

Later, we enforced the Quality Gate.

For example, our project required at least **80% coverage on the defined code scope**.

To make Jenkins aware of the Quality Gate result, we configured a **webhook from SonarQube to Jenkins**.

In Jenkins, we used:

```groovy
waitForQualityGate()
```

to wait for the analysis result.

If the Quality Gate passed, Jenkins continued to the next stages such as Docker build and deployment.

If it failed, Jenkins aborted the pipeline.

### End-to-End Flow

```text
Developer
    ↓
Git
    ↓
Jenkins
    ↓
Checkout
    ↓
Install Dependencies
    ↓
Unit Tests
    ↓
Coverage Report Generated
    ↓
SonarQube Scanner
    ↓
SonarQube Server
    ↓
Coverage + Bugs + Code Smells + Vulnerabilities
    ↓
Quality Gate
    ↓
 ┌──────────────┴──────────────┐
 ↓                             ↓
PASS                          FAIL
 ↓                             ↓
Docker Build                  Stop
 ↓
Deployment
```

So overall, our approach was to **generate coverage during testing, send it to SonarQube through the scanner, evaluate it through the Quality Gate, and use the webhook to control whether Jenkins should continue or stop.**

---

# 19. Can you give a specific example of a code smell and security vulnerability detected by SonarQube?

Yes.

## Code Smell Example – Duplicated Code

One example of a code smell is **duplicated code**.

For example, if the same business logic is copied into multiple methods, SonarQube can report it as duplicated code.

It makes the application difficult to maintain because if we need to change that logic, we have to modify it in multiple places.

We can fix it by extracting the common logic into a reusable method.

### Flow

```text
Same Logic
   ↓
Copied into Multiple Methods
   ↓
Code Duplication
   ↓
SonarQube Reports Code Smell
   ↓
Extract Common Logic
   ↓
Reusable Method
```

---

## Security Vulnerability Example – SQL Injection

For a security vulnerability, one example is **SQL injection**.

If application code directly concatenates user input into a SQL query, an attacker may be able to manipulate the query.

For example, insecure code may look conceptually like:

```text
SELECT * FROM users WHERE username = ' + userInput + '
```

SonarQube can identify insecure coding patterns like this.

We can fix it by using **parameterized queries or prepared statements**.

### Flow

```text
User Input
    ↓
Directly Added to SQL Query
    ↓
Potential SQL Injection
    ↓
SonarQube Detects Issue
    ↓
Use Parameterized Query
    ↓
Safer Application
```

### Interview summary

> "For a code smell, one example is duplicated code. It increases maintenance effort because the same logic exists in multiple places. For a security vulnerability, SQL injection is an example where untrusted user input is directly concatenated into SQL queries. We can fix it by using parameterized queries or prepared statements."

---

# Quick Revision – Important Commands

## Terraform

```bash
terraform init
terraform validate
terraform plan
terraform apply
terraform destroy
terraform force-unlock <LOCK_ID>
```

## Docker

```bash
docker build
docker run
docker ps
docker images
docker stop
docker rm
docker rmi
```

## SonarQube / Jenkins Flow

```text
Git
 ↓
Jenkins
 ↓
Checkout
 ↓
Install Dependencies
 ↓
Unit Test
 ↓
Coverage
 ↓
SonarQube Scan
 ↓
Quality Gate
 ↓
PASS → Docker Build → Deployment
FAIL → Pipeline Stops
```

---

# Interview Mindset

For scenario-based DevOps questions, I should explain:

```text
1. Identify the problem
        ↓
2. Check the current situation
        ↓
3. Find the root cause
        ↓
4. Take the appropriate action
        ↓
5. Validate the result
        ↓
6. Continue / prevent the issue
```

For example, for Terraform state locking:

```text
Problem
  ↓
State Locked
  ↓
Check Lock
  ↓
Check Running Pipeline
  ↓
Confirm Stale Lock
  ↓
Force Unlock
  ↓
Terraform Plan
  ↓
Validate
  ↓
Rerun Pipeline
```

22. Where are Terraform state/locking details stored when using DynamoDB with S3?
    Terraform stores the state file in an S3 bucket. DynamoDB is used to maintain the state lock, which prevents multiple Terraform operations from modifying the same state simultaneously. So, S3 stores the Terraform state, and DynamoDB handles the locking.
24. Why are locks and state files stored using S3 and DynamoDB in Terraform?
    Terraform state needs to be shared among team members, so we store the state file remotely in S3. Since multiple users or CI pipelines might run Terraform at the same time, we need state locking to prevent concurrent modifications. In the traditional S3 backend setup, DynamoDB is used for this locking mechanism. S3 provides centralized and durable state storage, while DynamoDB helps coordinate access to that state
26. What is the use of Prometheus and Grafana?
27. What kind of data/metrics are collected by Prometheus and Grafana?
28. What specific metrics does Prometheus capture, and what is the exact role of Grafana?
29. Why have you switched companies a few times over your six years of experience :rolling_on_the_floor_laughing: (maybw we frastrated ,just ignore )
30. What source code management tool do you use?
31. Is it GitLab or GitHub? Which one?
32. When you are using GitHub, how do you create a CI/CD pipeline?
33. What is the integration layer between GitHub and Jenkins?
34. What is the use of GitHub Actions?
35. How do you troubleshoot failed Jenkins pipelines, specifically around SonarQube Quality Gate failures?
