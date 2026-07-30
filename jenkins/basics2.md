## Jenkins (Interview Notes)

Jenkins is an **open-source automation tool** used for **CI/CD (Continuous Integration and Continuous Deployment)**.

---

## Why Jenkins is Used?

* To **automate build and deployment process**
* To reduce **manual work**
* To achieve **faster and reliable releases**

---

## What Jenkins Does?

Jenkins helps in automating the complete pipeline:

1. Developer pushes code to Git
2. Jenkins pulls the code
3. Builds the application
4. Runs tests
5. Deploys the application

---

## Key Concepts

* **Pipeline** – Defines the steps (build, test, deploy)
* **Job** – A single task or process
* **Plugins** – Used to integrate with tools like Git, Docker, Kubernetes
* **Agent/Node** – Where the job runs

---

## Real-Time Flow

Developer → Git → Jenkins → Build → Test → Deploy

---

## Key Interview Points

* Jenkins is a **CI/CD tool**
* It automates **build, test, and deployment**
* Helps in **continuous delivery**
* Reduces **human errors**
* Supports many plugins for integration

## Advantages of Jenkins (Interview Notes)

Jenkins provides several advantages in the software development process:

---

## Key Advantages

* **Automation**
  Automates build, test, and deployment processes, reducing manual effort.

* **Faster Delivery**
  Enables quick and continuous releases.

* **Open Source**
  Free to use with strong community support.

* **Extensible (Plugins)**
  Supports 1000+ plugins to integrate with tools like Git, Docker, Kubernetes.

* **Continuous Integration**
  Automatically builds and tests code whenever changes are pushed.

* **Continuous Deployment**
  Can automate deployment to different environments.

* **Reduces Human Errors**
  Automation ensures consistency and reliability.

* **Easy Integration**
  Works with most DevOps tools and cloud platforms.

---

## One-Line Answer

Jenkins helps automate CI/CD processes, making software delivery **faster, reliable, and error-free**.

## CI/CD (Interview Notes)

CI/CD stands for:

* **CI – Continuous Integration**
* **CD – Continuous Delivery / Continuous Deployment**

---

## Continuous Integration (CI)

* Developers frequently push code to a shared repository (like Git)
* Each change is automatically **built and tested**
* Helps to find bugs early

**Example:**
Whenever a developer pushes code → Jenkins runs build + tests automatically

---

## Continuous Delivery (CD)

* Code is automatically **prepared for release**
* Deployment to production is **manual approval**

---

## Continuous Deployment (CD)

* Code is automatically **deployed to production**
* No manual intervention

---

## Simple Flow

Developer → Git → Build → Test → Deploy

---

## Key Interview Points

* CI = **Build + Test automation**
* CD = **Deployment automation**
* Helps in **faster, reliable, and frequent releases**
* Reduces **manual work and errors**

---

## One-Line Answer

CI/CD is a process that automates **building, testing, and deploying applications** for faster and reliable delivery.

## Ways to Install Jenkins (Interview Notes)

There are multiple ways to install Jenkins based on the environment:

---

## 1. Using Package Manager (Recommended)

* Install using tools like:

  * **apt** (Ubuntu)
  * **yum** (RHEL/CentOS)

**Example:**
Install Jenkins directly from official repository.

---

## 2. Using WAR File

* Download Jenkins `.war` file
* Run using Java:

`java -jar jenkins.war`

* Simple and quick setup

---

## 3. Using Docker

* Run Jenkins as a container

**Example:**
`docker run -p 8080:8080 jenkins/jenkins`

* Easy to set up and manage

---

## 4. Using Kubernetes

* Deploy Jenkins on Kubernetes using:

  * YAML files
  * Helm charts

* Suitable for scalable environments

---

## 5. Using Cloud Platforms

* Install Jenkins on:

  * AWS EC2
  * Azure VM
  * Google Cloud VM

---

## Key Interview Points

* Multiple installation methods available
* Docker and Kubernetes are preferred in modern setups
* WAR file is simplest for quick testing
* Package manager is best for stable environments

---

## One-Line Answer

Jenkins can be installed using package managers, WAR file, Docker, Kubernetes, or on cloud virtual machines.

## Commands to Start Jenkins Manually (Interview Notes)

There are different ways to start Jenkins manually based on how it is installed:

---

## 1. Using WAR File

If Jenkins is running using a `.war` file:

```bash
java -jar jenkins.war
```

---

## 2. Using System Service (Linux)

If installed using package manager:

### Start Jenkins

```bash
sudo systemctl start jenkins
```

### Stop Jenkins

```bash
sudo systemctl stop jenkins
```

### Restart Jenkins

```bash
sudo systemctl restart jenkins
```

### Check Status

```bash
sudo systemctl status jenkins
```

---

## 3. Using Service Command (Older Systems)

```bash
sudo service jenkins start
```

---

## 4. Using Docker

If Jenkins is running in a container:

```bash
docker start <container_id>
```

---

## Key Interview Points

* `java -jar jenkins.war` → manual standalone run
* `systemctl start jenkins` → most common in servers
* Docker command used in container-based setups

---

## One-Line Answer

Jenkins can be started manually using `java -jar jenkins.war`, `systemctl start jenkins`, or Docker commands depending on the setup.

## Jenkins Pipeline (Interview Notes)

A Jenkins Pipeline is a **set of automated steps** that define the process of building, testing, and deploying an application.

---

## Simple Explanation

* It is used to **automate the CI/CD workflow**
* Written as code using a **Jenkinsfile**
* Defines all stages like build, test, and deploy

---

## Pipeline Stages

Common stages in a pipeline:

1. Build
2. Test
3. Deploy

---

## Types of Pipelines

* **Declarative Pipeline**
  Simple and structured syntax

* **Scripted Pipeline**
  More flexible, written in Groovy

---

## Example Flow

Code Commit → Build → Test → Deploy

---

## Why We Use Pipeline?

* Automates complete process
* Makes workflow **repeatable and reliable**
* Easy to track and debug

---

## Key Interview Points

* Pipeline = **automation of CI/CD steps**
* Defined in **Jenkinsfile**
* Supports **multiple stages**
* Improves **efficiency and consistency**

---

## One-Line Answer

A Jenkins Pipeline is a way to define and automate the **build, test, and deployment process** as code.

## Types of Jenkins Pipelines (Interview Notes)

There are mainly **two types of Jenkins Pipelines**:

1. Declarative Pipeline
2. Scripted Pipeline

---

## 1. Declarative Pipeline

* Simple and **easy-to-use syntax**
* Written in a structured format
* Recommended for most use cases

**Key Features:**

* Easy to read and maintain
* Uses predefined blocks like `pipeline`, `stages`, `steps`
* Less flexible but more user-friendly

---

## 2. Scripted Pipeline

* Written using **Groovy scripting**
* More **flexible and powerful**
* Suitable for complex workflows

**Key Features:**

* Full control over logic
* Can use conditions, loops, and custom logic
* Slightly harder to understand

---

## Key Differences

* Declarative → Simple, structured
* Scripted → Flexible, advanced

---

## Key Interview Points

* Two types: **Declarative and Scripted**
* Declarative is **recommended**
* Scripted is used for **complex logic**
* Both are written in a **Jenkinsfile**

---

## One-Line Answer

Jenkins pipelines are of two types: Declarative (simple and structured) and Scripted (flexible and powerful using Groovy).

## Default Environment Variables in Jenkins (Interview Notes)

Jenkins provides several **built-in environment variables** that can be used inside pipelines and jobs.

---

## Common Default Environment Variables

* **BUILD_ID**
  Unique ID of the current build

* **BUILD_NUMBER**
  Incremental number of the build

* **BUILD_URL**
  URL of the current build

* **JOB_NAME**
  Name of the Jenkins job

* **JOB_URL**
  URL of the job

* **WORKSPACE**
  Directory where the job is executed

* **NODE_NAME**
  Name of the node where the job is running

* **EXECUTOR_NUMBER**
  Executor number running the build

* **JENKINS_HOME**
  Jenkins home directory

* **GIT_COMMIT**
  Latest commit ID (if using Git)

* **GIT_BRANCH**
  Branch name

---

## How to Use

You can access variables in pipeline like:

```bash id="q7l5tp"
echo $BUILD_NUMBER
echo $JOB_NAME
```

---

## Key Interview Points

* Jenkins provides **default environment variables**
* Used to get **build, job, and system details**
* Helpful in **automation and scripting**

---

## One-Line Answer

Jenkins has built-in environment variables like BUILD_NUMBER, JOB_NAME, and WORKSPACE that provide information about the build and execution environment.

## Jenkins Pipeline – Real-Time Example (Interview Answer)

Let’s say I am working on a project where developers are building a web application.

---

## Real-Time Flow

1. Developer writes code and pushes it to Git (GitHub/GitLab)
2. Jenkins Pipeline is automatically triggered

---

## Pipeline Stages

* **Stage 1: Build**
  Jenkins pulls the latest code and builds the application

* **Stage 2: Test**
  Runs unit tests to check if the code is working fine

* **Stage 3: Docker Build**
  Creates a Docker image of the application

* **Stage 4: Push Image**
  Pushes the Docker image to Docker Hub or a registry

* **Stage 5: Deploy**
  Deploys the application to Kubernetes or a server

---

## What Happens Automatically?

* No manual build
* No manual testing
* No manual deployment

Everything is handled by the pipeline.

---

## Benefits in Real-Time

* Faster releases
* Zero/less downtime
* Early bug detection
* Reliable deployments

---

## One-Line Real-Time Answer

In real-time, Jenkins Pipeline automatically takes code from Git, builds it, tests it, creates a Docker image, and deploys it to production without manual intervention.

## Build Trigger in Jenkins (Interview Notes)

A build trigger in Jenkins is used to **automatically start a job or pipeline** based on certain conditions or events.

---

## Why Do We Use Build Triggers?

* To **automate job execution**
* To avoid manual triggering
* To enable **continuous integration**

---

## Types of Build Triggers

* **SCM Polling**
  Jenkins checks the repository for changes at regular intervals

* **Webhook Trigger**
  Triggered instantly when code is pushed (GitHub/GitLab)

* **Build Periodically**
  Runs the job at scheduled times (like a cron job)

* **Trigger After Another Build**
  Starts a job after another job is completed

* **Manual Trigger**
  User starts the build manually

---

## How to Configure Build Trigger

1. Go to Jenkins Dashboard
2. Select your Job / Pipeline
3. Click on **Configure**
4. Scroll to **Build Triggers** section
5. Choose the required trigger:

   * Poll SCM
   * Build periodically
   * GitHub webhook, etc.
6. Save the configuration

---

## Key Interview Points

* Build trigger = **automates job execution**
* Webhook is **real-time trigger (most preferred)**
* Poll SCM checks at intervals
* Can schedule builds using cron

---

## One-Line Answer

A build trigger in Jenkins is used to automatically start a job based on events like code changes, schedules, or other job completions.

## Jenkins Master vs Agent (Interview Notes)

---

## What is Jenkins Master?

* The **main server** of Jenkins
* Responsible for:

  * Managing jobs/pipelines
  * Scheduling builds
  * Monitoring execution
  * Providing UI (dashboard)

---

## What is Jenkins Agent?

* A **worker machine/node** connected to the master
* Responsible for:

  * Executing build jobs
  * Running pipelines
  * Performing tasks assigned by master

---

## Key Differences

| Master                       | Agent                    |
| ---------------------------- | ------------------------ |
| Controls and manages Jenkins | Executes jobs            |
| Schedules builds             | Runs builds              |
| Provides UI                  | No UI                    |
| Single main server           | Can have multiple agents |
| Handles coordination         | Handles execution        |

---

## Why Use Agents?

* To **distribute workload**
* To improve **performance and scalability**
* To run jobs on different environments (Linux, Windows, etc.)

---

## Simple Flow

Master → assigns job → Agent → executes job → returns result

---

## Key Interview Points

* Master = **brain (controller)**
* Agent = **worker (executor)**
* Improves **scalability and performance**

---

## One-Line Answer

Jenkins master manages and schedules jobs, while agents execute those jobs on different machines.

## Jenkins Agents – Real-Time Example (Interview Answer)

In our project, we used Jenkins with multiple agents to handle different types of workloads.

---

## Real-Time Scenario

We had a web application with backend (Java) and frontend (React), and we used different agents for different tasks.

---

## How Agents Were Used

* **Master Node**

  * Managed the pipeline
  * Scheduled jobs
  * Did not run heavy builds

---

### Agent 1 (Linux Agent)

* Used for backend builds
* Ran Maven build commands
* Executed unit tests

---

### Agent 2 (Docker Agent)

* Built Docker images
* Pushed images to Docker Hub

---

### Agent 3 (Kubernetes Agent)

* Deployed application to Kubernetes cluster
* Ran kubectl commands

---

## Flow

1. Developer pushes code to Git
2. Master triggers pipeline
3. Backend build runs on Linux agent
4. Docker build runs on Docker agent
5. Deployment runs on Kubernetes agent

---

## Why This Setup?

* Distributes workload across multiple machines
* Faster execution (parallel builds)
* Each agent is optimized for a specific task
* Improves scalability and performance

---

## One-Line Real-Time Answer

In real-time, Jenkins master schedules jobs, and different agents handle tasks like build, Docker image creation, and deployment, improving performance and scalability.

## Jenkins Agent Offline – Real-Time Scenario (Interview Answer)

---

## What Happens When an Agent is Offline?

* Jenkins master **cannot assign jobs** to that agent
* Any job configured for that agent will:

  * Stay in **queue (waiting state)**
  * Or **fail**, if no other agent is available

---

## Real-Time Example

In our project, we had a **Docker agent** responsible for building Docker images.

One day, the agent went offline due to a **server issue**.

### What Happened:

* Pipeline was triggered after code push
* Build stage (Linux agent) completed successfully
* Docker stage was stuck in **“waiting for agent”**
* Deployment got delayed

---

## Best Practices (What We Did)

* **Multiple Agents (High Availability)**
  We configured backup agents for critical stages

* **Agent Labeling**
  Instead of using a single agent, we used labels like:
  `docker-agent` → mapped to multiple nodes

* **Auto-Scaling (Cloud/K8s)**
  Used Kubernetes plugin to spin up agents dynamically

* **Monitoring & Alerts**
  Set up alerts when an agent goes offline

* **Fallback Strategy**
  Allowed jobs to run on another available agent

---

## Key Interview Points

* Offline agent → job will **wait or fail**
* Use **multiple agents** to avoid single point of failure
* Use **labels instead of hardcoding agents**
* Prefer **auto-scaling agents (Kubernetes/Docker)**

---

## One-Line Real-Time Answer

If a Jenkins agent is offline, jobs get stuck or fail, so in real-time we use multiple agents, labels, and auto-scaling to ensure high availability and avoid pipeline delays.

## Scopes of Jenkins Credentials (Interview Notes)

Jenkins credentials are used to securely store sensitive data like **passwords, API keys, SSH keys, tokens**, etc.

---

## Types of Credential Scopes

### 1. Global Scope

* Accessible by **all jobs and pipelines** in Jenkins
* Most commonly used
* Useful for shared credentials (e.g., GitHub, Docker Hub)

---

### 2. System Scope

* Accessible only by **Jenkins system/internal processes**
* Not available to jobs or pipelines
* Used for internal integrations

---

### 3. Folder Scope

* Accessible only within a **specific folder** in Jenkins
* Used to restrict access for teams/projects
* Improves security

---

## Why Scopes Are Important?

* Control **who can access credentials**
* Improve **security and isolation**
* Prevent unauthorized usage

---

## Key Interview Points

* 3 scopes: **Global, System, Folder**
* Global → accessible everywhere
* System → internal use only
* Folder → restricted to specific jobs

---

## One-Line Answer

Jenkins credentials have scopes like Global, System, and Folder to control access and improve security.
