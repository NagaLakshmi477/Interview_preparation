## What are Terraform workspaces?
Terraform workspaces allow us to manage multiple environments using the same Terraform configuration. Each workspace has its own Terraform state, so we can use the same code for environments like development, staging, and production while keeping their state separate.
Terraform Code
      |
      ├── dev workspace   → dev infrastructure
      ├── staging         → staging infrastructure
      └── prod            → prod infrastructure
terraform workspace new dev
terraform workspace select dev
terraform workspace list

## What are Terraform modules and why do we use them?
If we have the same infrastructure configuration across different projects or environments, instead of writing the same Terraform code again and again, we can create a module and reuse it. We can pass different variables to the module depending on our requirements.
Same Module
    ↓
 ┌───────┬─────────┬──────────┐
 Dev    Staging    Prod
  ↓        ↓         ↓
Small    Medium     Large

## How do you manage Terraform state in a team environment?
We manage Terraform state using a remote backend. For example, in AWS, we can store the Terraform state file in an S3 bucket. This allows the team to share the same state instead of keeping it locally. We also enable state locking so that two people cannot modify the infrastructure at the same time.
Developer 1 ──┐
              ├──→ Terraform ──→ S3
Developer 2 ──┘                  │
                                 ↓
                            State file

## What is Terraform state management?
Terraform state management is the process of maintaining the Terraform state file. In a team environment, instead of keeping the state file locally, we store it in a remote backend such as S3. This gives all developers a shared state. If every developer keeps a separate local state file, the state can become inconsistent and cause infrastructure management problems. With remote state, everyone works with the same state, and state locking prevents multiple developers from modifying it at the same time.


## What repetitive tasks have you automated using Shell scripting?
Imagine you have a Linux server running your application.

Every day, someone might have to do things like:
Check disk space
      ↓
Check application status
      ↓
Check logs
      ↓
Delete old logs
      ↓
Restart service if required
If a person does this manually every day, it's repetitive.

Instead, we can write a Bash script that performs these commands automatically.

We can create a script:
#!/bin/bash

LOG_DIR="/app/logs"

echo "Checking disk usage..."
df -h

echo "Deleting logs older than 7 days..."
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -delete

echo "Disk usage after cleanup..."
df -h
             Linux Server
                  │
                  ↓
              Cron Job
                  │
                  ↓
        cleanup_logs.sh
                  │
          ┌───────┴────────┐
          ↓                ↓
    Check disk        Delete old logs
          │                │
          └───────┬────────┘
                  ↓
            Disk cleaned
One repetitive task I automated was log cleanup. We had application logs consuming disk space, so I created a Bash script to check disk usage and remove old log files based on the retention period. I scheduled the script using a cron job so it ran automatically. This reduced manual server maintenance.

##  Have you worked on incident management / production incidents?

Yes, I have worked on production incidents and troubleshooting.

In my current role, whenever there is an issue in the application or infrastructure, I first try to understand the impact and identify whether the issue is related to the application, infrastructure, networking, database, or Kubernetes.

For example, if a deployment is successful but the application is not accessible, I would check the Kubernetes pods and their status, application logs, services, ingress or load balancer configuration, security groups, and network connectivity.

If it is a pod-related issue, I check events and logs to understand whether the problem is related to configuration, resource usage, image issues, or application startup.

Once I identify the root cause, I work on the fix, validate the application, and monitor it to make sure the issue is resolved.

I have also worked on troubleshooting database connectivity and application startup issues across different environments.

So, my approach during production incidents is basically: understand the impact, identify the root cause, fix the issue, validate the solution, and monitor the application after the fix.


## How does a request flow from a browser to a Kubernetes pod?
user opened https://myapp.com
User's Browser
      ↓
     DNS
      ↓
Load Balancer
      ↓
   Ingress
      ↓
   Service
      ↓
 ┌────┴────┐
 ↓         ↓
Pod 1     Pod 2
           ↓
      Application

Kubernetes concept	Simple meaning
Browser	            You make the request
DNS	                  Finds the application's address
Load Balancer	       Entry point for incoming traffic
Ingress	            Decides where HTTP/HTTPS traffic should go
Service	            Provides a stable way to reach Pods
Pod	                  Runs your application
Application	Actually processes the request

##  What happens internally when you run a Docker container?
docker run nginx
       ↓
Check local images
       ↓
Image not found
       ↓
Pull image from registry
Once the image is available, Docker uses that image to create a container.

## How does Kubernetes decide which node should run a pod?
We have 3 worker nodes:
Node 1 → 4 CPU, 8 GB RAM
Node 2 → 4 CPU, 8 GB RAM
Node 3 → 4 CPU, 8 GB RAM
Now we create a Pod:
Pod
CPU    → 1 CPU
Memory → 2 GB
The Pod doesn't decide where to run. The Kubernetes Scheduler makes this decision.
You create Pod
      ↓
API Server receives the request
      ↓
Scheduler notices the Pod has no node
      ↓
Scheduler checks available nodes
      ↓
Filters unsuitable nodes
      ↓
Chooses the best suitable node
      ↓
Pod is assigned to that node
      ↓
Kubelet on that node starts the Pod

Scheduler → decides WHERE the Pod should run

Kubelet   → makes sure the Pod actually RUNS on that node

## Difference between Readiness, Liveness, and Startup Probes.
Suppose we have a Java application running inside a Pod:
Pod
 └── Java Application

 Kubernetes needs to know:

Is the application ready to receive traffic?
Is the application still alive or stuck?
Has the application finished starting?

That's where these three probes are used.
The key difference:
Readiness → traffic
Liveness → restart
Startup → give slow applications time to start

ex:
             Service
                |
       ┌────────┼────────┐
       ↓        ↓        ↓
     Pod 1    Pod 2    Pod 3
     Ready    Ready    Starting
Pod 3 is starting

Startup probe says:

"Give Pod 3 some time."

Readiness says:

"Pod 3 is not ready yet.
Don't send traffic."

Liveness says:

"Don't restart Pod 3 just because it hasn't
finished starting yet."

## Why would a pod be Running but the application still be unavailable?

Pod → Running ✅
We might think the application must be working, but Running only tells us that the Pod/container is running. It doesn't necessarily mean the application is ready to serve requests.
Pod
 └── Container → Running
       └── Java application → ❌ not responding
- Application itself has a problem
The container is running, but the Java application may be stuck or not responding.
Pod → Running ✅
Application → Not responding ❌
So the application is unavailable even though the Pod shows Running.
- Service is not pointing to the Pod
Service selector might not match the Pod labels.      
Service selector:
app: myapp

Pod label:
app: payment
They don't match.

So the Service doesn't find the Pod.

Service
   ↓
No matching endpoint ❌
- Wrong port
How would I troubleshoot it?
kubectl get pods
kubectl logs <pod-name>
kubectl describe pod <pod-name>
kubectl get svc
kubectl get endpoints <service-name>

## Explain the complete lifecycle of a CI/CD pipeline from commit to production.

## How does Terraform build and execute its dependency graph?
Terraform configuration
        ↓
Understand resources
        ↓
Find dependencies
        ↓
Build dependency graph
        ↓
Create/update resources
        ↓
Independent resources can run in parallel
        ↓
Dependent resources wait

## What is Terraform Drift, and how do you handle it?
Terraform Drift means the actual infrastructure has changed outside of Terraform, so it is different from what Terraform expects based on its configuration/state.
Terraform
   ↓
EC2
CPU = t3.medium
Later, someone manually goes to AWS and changes it:
Actual AWS:
EC2
CPU = t3.large
But our Terraform configuration still says: t3.medium

Terraform configuration → t3.medium
Actual AWS              → t3.large
                              ↑
                            Drift

How do I find the drift? terraform plan
Terraform state file compares actula infra and desried infra

## How does Terraform state locking work with S3 and DynamoDB?
## How do you design reusable Terraform modules for enterprise projects?
## Explain the difference between Rolling, Blue-Green, and Canary deployments.
##  How would you implement a zero-downtime deployment?
see k8s notes
