# hat is the difference between terraform refresh and terraform plan?
- refresh command is updates the state file
- plan commands check the state file desired state with actual state
# What are Lifecycle Hooks in Terraform?

* Lifecycle hooks control **how Terraform creates, updates, and deletes resources**.
* They help us **avoid downtime** and **prevent accidental deletion**.
* These are rules defined in Terraform that control how a resource behaves during updates or deletion.

## Examples

### `create_before_destroy = true`

* Terraform will **create the new resource first, then delete the old one**
* Used to **avoid downtime**

### `prevent_destroy = true`

* This resource **cannot be deleted by Terraform**
* Protects critical resources like **databases or production systems**
  
# Terraform Apply Fails — How do you debug?

- If `terraform apply` fails, first I carefully read the error message because Terraform usually gives a clear idea of what went wrong.

- Then I run `terraform plan` again to understand what changes it was trying to make.

- I check the `.terraform` directory to ensure plugins and providers are properly initialized.

- Next, I verify if there are any permission issues (like AWS IAM roles) or quota limits.

- I also validate for any provider version mismatches.

- If needed, I enable debug logs using:
  TF_LOG=DEBUG terraform apply

- Finally, I fix the issue and re-run the apply.
# What is terraform fmt and terraform validate
- terraform fmt  Formats the code
- terraform validate Checks syntax and logical correctnes
  
# What is EKS Module?

- EKS (Elastic Kubernetes Service) allows us to run Kubernetes on AWS without managing the control plane.

- AWS takes care of cluster management, scaling, and availability, while we focus on deploying applications.

- The Terraform EKS module is a reusable configuration that simplifies creating and managing an EKS cluster.

## Steps to Provision EKS using Terraform

- Configure AWS provider  
- Create VPC & subnets  
- Use EKS module  
- Define node groups  
- Configure IAM roles  
- Run Terraform commands (`init`, `plan`, `apply`)  
- Connect using kubeconfig
# How to connect Terraform-managed EC2 with RDS securely?
- RDS is placed in a private subnet, and access is controlled using security groups
-  so only EC2 can connect on the required port, using the DB endpoint from Terraform outputs
## points
- Create RDS in private subnet
- Attach EC2 in public subnet with SG allowing DB port 3306 to RDS SG only
- Use variables for DB endpoint from Terraform output
# How do you handle sensitive data in Terraform?

- Use `sensitive = true` for variables so Terraform does not expose values in logs or output.

- Store secrets in secure services like:
  - AWS Secrets Manager
  - Azure Key Vault
  - HashiCorp Vault

- Never commit sensitive files like:
  - `.tfstate`
  - `.tfvars`

- Use remote backends (like S3 with encryption or Terraform Cloud) to securely store state.

- Integrate Terraform with secret management tools (e.g., Terraform Cloud + Vault).

## Example

variable "db_password" {
  type      = string
  sensitive = true
}

# What is a Root Module and Child Module?

- **Root Module**:
  - The main Terraform configuration directory
  - This is where we run commands like `terraform init`, `plan`, and `apply`

- **Child Module**:
  - Reusable modules that are called from the root module using the `module` block
  - Used to organize and reuse infrastructure code

## Why use this structure?

- Helps in better organization of code  
- Promotes reusability  
- Allows separation of environments like dev, stage, and prod  

# How to Force Resource Recreation in Terraform?

- Use `terraform taint <resource>`
  - Marks a resource for recreation on the next `apply`
  - Terraform will destroy and recreate that resource

- In newer Terraform versions, use:
  terraform apply -replace=<resource>
  - Example:
    terraform apply -replace=aws_instance.web
# A DevOps Engineer manually created infrastructure on AWS, and now there is a requirement to use Terraform to manage it. How would you import these resources in Terraform code?

- First, create a Terraform resource block for the AWS resource.
- Run the `terraform import` command with the resource ID.
- Repeat this process for all the existing resources.
- Finally, run `terraform plan` to make sure Terraform is managing the resource correctly.

```bash
terraform import aws_instance.example i-1234567890abcdef0
```
# A Jr DevOps Engineer accidentally deleted the Terraform state file. What steps should we take to resolve this?

1. **Recover Backup**: If available, restore the state file from a recent backup.
2. **Recreate State**: If no backup exists, manually reconstruct the state by inspecting the existing infrastructure and using `terraform import` for the resources.
3. **Review and Prevent**: Analyze the incident, implement preventive measures, and educate the team on best practices to avoid similar issues in the future.
   
# What are some best practices to manage the Terraform state file?

1. **Remote Storage**: Store the state file remotely (e.g., AWS S3) for safety, collaboration, and version control.
2. **State Locking**: Enable state locking to prevent conflicts during concurrent operations.
3. **Access Controls**: Restrict access to the state file to authorized users and services.
4. **Automated Backups**: Configure automated backups to prevent data loss.
5. **Environment Separation**: Maintain separate state files for each environment or use Terraform workspaces to manage multiple state files.
   
# Your company wants to automate Terraform through CI/CD pipelines. How can you integrate Terraform with CI/CD pipelines?

- First, I store the Terraform code in a Git repository.
- Whenever a developer pushes code, the CI/CD pipeline (like Jenkins or GitHub Actions) is triggered automatically.
- The pipeline runs `terraform init` to initialize Terraform.
- Then it runs `terraform validate` to check for syntax errors.
- Next, it runs `terraform plan` to show what infrastructure changes will be made.
- After the changes are reviewed and approved (if required), the pipeline runs `terraform apply` to create or update the infrastructure.
- Once the infrastructure is ready, the pipeline continues with application deployment and testing.

# You have an RDS Database and an EC2 instance. EC2 should be created before RDS. How can you specify dependencies between resources in Terraform?

- Terraform automatically creates resources in the correct order when one resource references another.
- If there is no direct reference, we can use the `depends_on` argument to create an explicit dependency.
- In this case, we can make the RDS resource depend on the EC2 instance using `depends_on`.

```hcl
resource "aws_db_instance" "rds" {
  depends_on = [aws_instance.ec2]
}
```

**Interview Answer:**

> If there is a reference between resources, Terraform automatically handles the dependency. If there is no reference, I use the `depends_on` argument to explicitly tell Terraform to create the EC2 instance first and then create the RDS instance.

# You have 20 servers created through Terraform, but you want to delete only one of them. Is it possible to destroy a single resource using Terraform?

- Yes, it is possible.
- We can use the `terraform destroy -target` command to destroy only the specific resource.
- Terraform will remove only the targeted resource without affecting the other resources.

```bash
terraform destroy -target=aws_instance.web
```

**Interview Answer:**

> Yes. Terraform allows us to destroy a specific resource using the `terraform destroy -target` command. This removes only the selected resource while leaving the remaining infrastructure unchanged.
