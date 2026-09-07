ANG
==========
1. Your Terraform pipeline is failing because Terraform State is locked. What would you do?
   If my Terraform pipeline fails because the state is locked, first I would not immediately force-unlock it. I would check whether another Terraform operation or Jenkins pipeline is currently running against the same state.

I would check the lock information, including the lock ID, user, operation and timestamp. If another Terraform operation is running, I would wait for it to complete.

If I confirm that the lock is stale because a previous pipeline failed or was terminated unexpectedly, I would use `terraform force-unlock <LOCK_ID>` to remove the stale lock.

After unlocking, I would run `terraform plan` to verify that the state and infrastructure are consistent, and then rerun the pipeline.

The important point is that I would use `force-unlock` only after confirming that no other Terraform operation is running.
terraform force-unlock <LOCK_ID>

2. How do you manage secrets in Terraform?
I don't hardcode secrets directly in Terraform code. I prefer storing secrets in a secure service such as AWS Secrets Manager or SSM Parameter Store and retrieving them when required.

I also mark Terraform variables containing sensitive information as `sensitive = true`, so the values are not exposed in Terraform output.

Since Terraform can store sensitive values in the state file, I secure the remote state as well. For AWS, I use an S3 backend with encryption and proper IAM permissions, and restrict access to the state file.

I also make sure that `.tfvars` files containing secrets are not committed to Git.
variable "db_password" {
  type      = string
  sensitive = true
}
Add it to .gitignore:
*.tfvars
*.tfstate
*.tfstate.*
3. Where do you save those secrets?
I save secrets in AWS Secrets Manager or SSM Parameter Store. I don't hardcode them in Terraform files or commit them to Git.

For example, database passwords and API keys can be stored in Secrets Manager, and Terraform or the application can retrieve them when required. I also make sure access is controlled using IAM permissions.

5. How do you retrieve RDS database passwords and things like that?
 For RDS credentials, I don't hardcode the database password in Terraform. I store the credentials in AWS Secrets Manager.

In Terraform, I use the Secrets Manager data source to retrieve the secret and use the username and password while creating or managing the RDS instance.

I also secure the Terraform remote state because sensitive values can be stored in the state file. I use encryption and proper IAM permissions to restrict access to the state.

7. What is the difference between Terraform and AWS CloudFormation?
8. What is Terraform Apply?
   Terraform Code (.tf)
       ↓
terraform plan
       ↓
Compare:
Desired state ↔ Current state
       ↓
Show changes
       ↓
Ask for confirmation
       ↓
terraform apply
       ↓
AWS API
       ↓
EC2 created/modified
       ↓
Terraform State updated
`terraform apply` is used to actually create, modify, or delete infrastructure according to the Terraform configuration.

When we run `terraform apply`, Terraform compares the desired configuration with the current state, creates an execution plan, and after approval it makes the required changes through the cloud provider APIs. After the changes are completed, Terraform updates the state file.

For example, if I define an EC2 instance in my Terraform code and run `terraform apply`, Terraform will create that EC2 instance in AWS.

The main difference is that `terraform plan` only shows what changes will happen, whereas `terraform apply` actually performs those changes.

10. How do you maintain Terraform files for the Dev environment and Production environment?
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
    I maintain Dev and Production environments separately, but I avoid duplicating the Terraform code.

I create reusable Terraform modules for common infrastructure such as VPC, EC2, RDS and EKS. Then I have separate environment directories for Dev and Prod, where I provide environment-specific variables and configuration.

For example, Dev may use smaller instance types and fewer replicas, while Production uses larger instances and higher availability.

I also maintain separate Terraform state for each environment, usually using separate paths in an S3 backend.

This approach gives us reusable code, environment isolation, and makes it easier to manage Dev and Production independently.
Terraform Code
      ↓
  Reusable Modules
      ↓
 ┌──────────────┐
 │              │
DEV            PROD
 │              │
 ↓              ↓
dev.tfstate    prod.tfstate
12. What is the difference between a Container and a VM ?
13. What is CMD and what is ENTRYPOINT in Docker?
14. What is SonarQube and what is the use of it? (they dont want genenric ans for sonarQube: he strictly looking end to end flow )
15. What is a code smell?
16. What is code coverage?
17. Can you give a specific example of a code smell  and  security vulnerability detected by SonarQube?
18. Explain how SonarQube is integrated into your Jenkins CI/CD pipeline and what metrics it checks.
19. Where are Terraform state/locking details stored when using DynamoDB with S3?
20. Why are locks and state files stored using S3 and DynamoDB in Terraform?
21. What is the use of Prometheus and Grafana?
22. What kind of data/metrics are collected by Prometheus and Grafana?
23. What specific metrics does Prometheus capture, and what is the exact role of Grafana?
24. Why have you switched companies a few times over your six years of experience :rolling_on_the_floor_laughing: (maybw we frastrated ,just ignore )
25. What source code management tool do you use?
26. Is it GitLab or GitHub? Which one?
27. When you are using GitHub, how do you create a CI/CD pipeline?
28. What is the integration layer between GitHub and Jenkins?
29. What is the use of GitHub Actions?
30. How do you troubleshoot failed Jenkins pipelines, specifically around SonarQube Quality Gate failures?
