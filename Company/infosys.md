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
## Give a real-time example of automation you implemented.



## What repetitive tasks have you automated using Bash/Shell scripting?
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
## Which scripting languages have you worked with?
##  Have you worked on incident management / production incidents?