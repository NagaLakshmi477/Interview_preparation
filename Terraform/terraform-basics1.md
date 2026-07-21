## Terraform interview Questions
## What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool developed by **HashiCorp**.

- It allows you to **create infrastructure** using code.
- It allows you to **manage infrastructure** using code.
- It allows you to **update infrastructure** using code.
- It allows you to **delete infrastructure** using code.
- It eliminates the need to manually create resources through the cloud provider's console.

## What language does Terraform use?

**Answer:**

Terraform uses **HCL (HashiCorp Configuration Language)**, which is a **human-readable** and **declarative** language used to define infrastructure resources.

- Easy to read and write.
- Human-readable syntax.
- JSON-compatible.
- Used to define infrastructure resources such as EC2 instances, VPCs, Security Groups, and Load Balancers.
## What are the main Terraform commands and workflow?

### Terraform Workflow

```text
Write Terraform Configuration (.tf files)
            ↓
      terraform init
            ↓
   terraform validate
            ↓
      terraform plan
            ↓
      terraform apply
            ↓
Infrastructure is Created/Updated
            ↓
(Optional)
    terraform destroy
```

---

## Main Terraform Commands

### 1. `terraform init`

- Initializes the Terraform working directory.
- Downloads the required provider plugins.
- Creates the `.terraform` directory.

**Example:**

```bash
terraform init
```

---

### 2. `terraform validate`

- Validates the Terraform configuration files.
- Checks for syntax errors and configuration issues.
- Does not create any infrastructure.

**Example:**

```bash
terraform validate
```

---

### 3. `terraform plan`

- Shows a preview of the changes Terraform will make.
- Displays which resources will be created, modified, or destroyed.
- Does not make any actual changes.

**Example:**

```bash
terraform plan
```

---

### 4. `terraform apply`

- Creates or updates the infrastructure.
- Executes the changes shown in the plan.
- Updates the Terraform state file.

**Example:**

```bash
terraform apply
```

---

### 5. `terraform destroy`
    
- Deletes all resources managed by Terraform.
- Used to clean up infrastructure when it is no longer needed.

**Example:**

```bash
terraform destroy
```

---
## Explain the "Desired State vs Actual State" concept.

### Answer

- **Desired State:** It is the infrastructure that I want Terraform to create or maintain, which I define in my `.tf` files.
- **Actual State:** It is the infrastructure that is currently running in the cloud.
- Terraform compares what I want (desired state) with what is currently available (actual state).
- If there is any difference, Terraform identifies it.
- It then creates, updates, or deletes only the required resources to make the infrastructure match my configuration.
  
## What is Idempotency in Terraform?

**Answer:**

> **Idempotency means running `terraform apply` multiple times without any configuration changes will not recreate or modify existing resources.**

## what are Resources in Terraform?
- Resources are the core components in Terraform that represent infrastructure objects such as virtual machines, storage, networks, and databases.

## What are Data Sources?
- Data Sources in Terraform allow you to fetch information about existing infrastructure or external resources without creating or modifying them
# Resource vs Data Source in Terraform

| Feature | Resource | Data Source |
|---------|----------|-------------|
| Purpose | Creates, updates, and deletes infrastructure. | Reads information about existing infrastructure. |
| Managed by Terraform | Yes | No |
| Declaration | `resource` block | `data` block |
| Operation | Creates or modifies infrastructure | Read-only (does not modify infrastructure) |
| State | Stored and managed in Terraform state | Information is read from existing resources |
| Use Case | Provision new resources such as EC2, VPC, or S3 buckets | Retrieve existing resources such as AMIs, VPCs, or Security Groups |
| Example | `resource "aws_instance" "web" {}` | `data "aws_ami" "latest" {}` |

## Example: Resource

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
}
```

## Example: Data Source

```hcl
data "aws_ami" "latest" {
  most_recent = true

  owners = ["amazon"]
}
```

## Interview Answer

**A Resource is used to create, update, and manage infrastructure, whereas a Data Source is used to retrieve information about existing infrastructure without creating or modifying it. Resources are managed by Terraform, while Data Sources are read-only and are used to reference existing resources.**

# What is Terraform State?

**Terraform State** is like **Terraform's memory**.

When Terraform creates infrastructure (such as a VM, database, or network), it saves information about those resources in a state file called **`terraform.tfstate`**.

This state file helps Terraform track the resources it has created. It also stores metadata such as resource IDs, dependencies, and the current state of the infrastructure.

Without the state file, Terraform wouldn't know which resources belong to your project or what changes need to be made.

## What are the types of State Backends in Terraform?

- A **State Backend** is the place where Terraform stores its **state file (`terraform.tfstate`)**.
- There are two types of state backends:

### 1. Local Backend (Default)

- Terraform stores the state file on your local machine.
- The state file is usually named **`terraform.tfstate`**.
- Not recommended for teams because each team member has their own copy of the state file, which can lead to conflicts.

### 2. Remote Backend

- Terraform stores the state file in a remote location instead of your local machine (for example, an **AWS S3 bucket**).
- This allows multiple team members to share the same state file.
- It also supports **state locking**, backups, and better collaboration.

## What is State Locking?

**State Locking** is a feature that prevents multiple people from modifying the same Terraform state file at the same time.

For example, if one person is running `terraform apply` to create or update resources, Terraform locks the state file.

If another person tries to run `terraform apply` at the same time, they cannot modify the state because it is locked. They have to wait until the first operation is completed.

Once the operation finishes, Terraform releases the lock, and then others can run `terraform apply`.

### One-line Definition

> **State Locking prevents multiple users from modifying the Terraform state file at the same time by allowing only one Terraform operation to update the state.**


## 6) How to Inspect Terraform State?

Terraform provides the following commands to inspect the state file:

| Command | Description |
|---------|-------------|
| `terraform show` | Displays the current state of your infrastructure and shows all the resources managed by Terraform. |
| `terraform state list` | Lists all the resources that Terraform is currently managing. |
| `terraform state show <resource>` | Displays detailed information about a specific resource in the state. |

### Example

```bash
terraform show

terraform state list

terraform state show aws_instance.web
```

### Interview Answer

> **To inspect the Terraform state, we use:**
> - `terraform show` – Displays the current state.
> - `terraform state list` – Lists all the resources in the state.
> - `terraform state show <resource>` – Shows detailed information about a specific resource.

## 18) How to Pass Variables in Terraform?

Terraform variables can be passed in several ways.

| Method | Description | Example |
|--------|-------------|---------|
| Variable Default | Define a default value in `variables.tf`. | `default = "t2.micro"` |
| Command Line (`-var`) | Pass the variable while running Terraform. | `terraform apply -var="instance_type=t2.small"` |
| Variable File (`.tfvars`) | Store variables in a separate file and pass it during execution. | `terraform apply -var-file="terraform.tfvars"` |
| Environment Variable | Set variables using environment variables. | `export TF_VAR_instance_type=t2.micro` |

### Example

**variables.tf**

```hcl
variable "instance_type" {
  type    = string
  default = "t2.micro"
}
```

**Using `-var`**

```bash
terraform apply -var="instance_type=t2.small"
```

**Using `terraform.tfvars`**

```hcl
instance_type = "t2.small"
```

```bash
terraform apply -var-file="terraform.tfvars"
```

**Using Environment Variable**

```bash
export TF_VAR_instance_type=t2.micro
terraform apply
```

### Interview Answer

> **Terraform variables can be passed using default values, the `-var` option, a `.tfvars` file, or environment variables (`TF_VAR_<variable_name>`).**

## What are Output Values?
- Output values are used to display important information about the resources created by Terraform
- such as public IP addresses, resource IDs, or endpoints, after running terraform apply

