# Day 1 

# Terraform Basics - Introduction and Notes

## 1. Introduction to Infrastructure as Code (IAC)

### What is IAC?
Infrastructure as Code (IAC) is a practice of managing and provisioning infrastructure through machine-readable configuration files. It allows developers and DevOps teams to automate infrastructure tasks, ensuring consistency and reducing manual effort.

### Benefits of IAC:
- **Consistency**: Ensures consistent configurations across environments.
- **Scalability**: Simplifies scaling infrastructure up or down.
- **Efficiency**: Reduces manual effort by automating repetitive tasks.
- **Version Control**: Stores infrastructure configurations in source control systems like Git.

---

## 2. Why We Need IAC

### Difference Between Shell Script, Ansible, and IAC Tools

| **Aspect**             | **Shell Script**                 | **Ansible**                       | **IAC Tools (e.g., Terraform)**  |
|------------------------|----------------------------------|------------------------------------|-----------------------------------|
| **Purpose**           | Task automation                 | Configuration management          | Infrastructure provisioning      |
| **State Management**  | No state management             | Limited state awareness           | Tracks desired state (e.g., Terraform state) |
| **Ease of Use**       | Requires manual updates         | Declarative and user-friendly     | Declarative, focusing on desired state       |
| **Idempotence**       | Difficult to achieve            | Built-in idempotence              | Ensures state matches configuration          |
| **Tool Type**         | Scripting                       | Configuration management          | Full-stack IAC                    |

---

## 3. Introduction to Terraform

### What is Terraform?
Terraform is an open-source IAC tool developed by HashiCorp. It enables users to define and provision infrastructure using a declarative configuration language.

### Key Features:
- **Provider Support**: Works with multiple cloud providers like AWS, Azure, GCP, etc.
- **Declarative Language**: Focus on "what" to build, not "how."
- **State Management**: Maintains a state file to track infrastructure changes.
- **Plan & Apply**: Allows users to preview changes with `terraform plan` before applying them.

---

## 4. Terraform Language (Basic Syntax)

Terraform uses HashiCorp Configuration Language (HCL), which is easy to read and write. Below is an overview of its basic syntax.

### Example Terraform Configuration:
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### Key Features of Syntax:
- **Blocks**: Enclosed in `{}` to define configurations.
- **Key-Value Pairs**: Used to specify attributes.
- **Comments**: Single-line comments with `#` or `//`.

---

## 5. Blocks Used in Terraform Language

### Common Blocks:

1. **Provider Block**:
   Specifies the cloud provider (e.g., AWS, Azure, GCP).
   ```hcl
   provider "aws" {
     region = "us-east-1"
   }
   ```

2. **Resource Block**:
   Defines infrastructure components like instances or storage.
   ```hcl
   resource "aws_instance" "example" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = "t2.micro"
   }
   ```

3. **Variable Block**:
   Allows parameterizing configurations.
   ```hcl
   variable "instance_type" {
     default = "t2.micro"
   }
   ```

4. **Output Block**:
   Displays results after applying configurations.
   ```hcl
   output "instance_id" {
     value = aws_instance.example.id
   }
   ```

5. **Data Block**:
   Fetches information about existing resources.
   ```hcl
   data "aws_ami" "example" {
     most_recent = true
     owners      = ["self"]
   }
   ```

---

## Notes for Students:
- Terraform is **cloud-agnostic**, making it a powerful tool for multi-cloud setups.
- Always use **version control systems** (like Git) for managing Terraform files.
- Practice writing simple configurations and gradually add complexity as you learn.
- Remember to follow best practices such as using modules and storing sensitive data in secure places (e.g., Terraform Vault).

---

### Additional Resources:
- Official Documentation: [Terraform by HashiCorp](https://www.terraform.io/)
- Hands-on Labs: [Katacoda Terraform Scenarios](https://www.katacoda.com/)
- Community Forum: [Discuss HashiCorp](https://discuss.hashicorp.com/)

-----

# Day 2

## 1. Install Terraform in a Linux System

Terraform can be installed on a Linux system using the following steps:

### Step 1: Download Terraform
```bash
wget https://releases.hashicorp.com/terraform/$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r '.current_version')/terraform_$(curl -s https://checkpoint-api.hashicorp.com/v1/check/terraform | jq -r '.current_version')_linux_amd64.zip
```

### Step 2: Unzip the Binary
```bash
sudo apt install unzip -y
unzip terraform_*_linux_amd64.zip
```

### Step 3: Move the Binary to PATH
```bash
sudo mv terraform /usr/local/bin/
```

### Step 4: Verify Installation
```bash
terraform -v
```

---

## 2. Write Your First Terraform Script to Deploy an EC2 Instance

### Step 1: Initialize a Directory
Create a directory for your Terraform project:
```bash
mkdir terraform-ec2
cd terraform-ec2
```

### Step 2: Create a `main.tf` File
Add the following configuration to the `main.tf` file:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0" # Example AMI ID
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-Example-Instance"
  }
}
```

### Step 3: Initialize Terraform
```bash
terraform init
```

### Step 4: Plan the Deployment
```bash
terraform plan
```

### Step 5: Apply the Deployment
```bash
terraform apply
```

### Step 6: Verify the EC2 Instance
Go to the AWS Management Console and verify the EC2 instance is running.

---

## 3. Explain Terraform Lifecycle

The Terraform lifecycle consists of several stages that manage the infrastructure's state effectively:

### 1. **Write**
Define infrastructure using Terraform configuration files.

### 2. **Init**
Initialize the working directory with `terraform init`. This sets up the backend and downloads provider plugins.

### 3. **Plan**
Generate an execution plan using `terraform plan` to preview the changes Terraform will make.

### 4. **Apply**
Apply the changes to achieve the desired state with `terraform apply`.

### 5. **Refresh**
Sync the Terraform state with the real-world infrastructure using `terraform refresh`.

### 6. **Destroy**
Remove all managed resources using `terraform destroy`.

---

## 4. Explain Different Files Created in Terraform Lifecycle

### 1. **main.tf**
Contains the core configuration for resources, providers, and variables.

### 2. **variables.tf**
Defines input variables to make the configuration reusable and flexible.

### 3. **outputs.tf**
Defines outputs to display information about your resources after deployment.

### 4. **terraform.tfstate**
A JSON file that stores the current state of infrastructure.

### 5. **terraform.tfstate.backup**
A backup of the previous state file.

### 6. **.terraform/**
A hidden directory containing provider plugins and other metadata.

---
## Refer Diagram:

![k8s Diagram](images/2025-01-09%2008.48.18.gif)

---

## Additional Notes
- Always review the `terraform plan` output before applying changes.
- Use version control (e.g., Git) to manage your Terraform configurations.
- Secure sensitive data using Terraform variables and secret management tools.

# Day-3 : Terraform Script to Deploy Security Group with HEREDOC in UserData

This guide covers the deployment of a Security Group using Terraform and explains the HEREDOC concept in UserData along with the key blocks in the script.

---

## 1. Introduction to Security Groups
A **Security Group** acts as a virtual firewall for your instance to control inbound and outbound traffic. Terraform allows you to define and manage Security Groups using Infrastructure as Code.

---

## 2. Terraform Script for Security Group

### Script
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_security_group" "web_sg" {
  name_prefix = "web-sg-"
  description = "Allow inbound HTTP and SSH traffic"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "web-sg"
  }
}
```

---

## 3. HEREDOC in UserData

### What is HEREDOC?
HEREDOC (**Here Document**) is a multi-line string syntax in Terraform used to define large blocks of text or commands. It is often utilized in `UserData` to pass startup scripts to cloud instances.

### Example with UserData
Below is an example of using HEREDOC within an EC2 instance resource:

```hcl
resource "aws_instance" "web_server" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"

  user_data = <<-EOF
    #!/bin/bash
    echo "Hello, World!" > /var/www/html/index.html
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
  EOF

  tags = {
    Name = "web-server"
  }
}
```

### Another Example:

```
user_data = <<EOF
  ${file("index.sh")}
  EOF
```

### HEREDOC Syntax
- **`<<-EOF`**: Begins the HEREDOC. The `-` allows indentation.
- **Content**: The script or text.
- **`EOF`**: Ends the HEREDOC block.

---

## 4. Key Blocks in the Terraform Script

### Provider Block
The `provider` block specifies the cloud provider to manage resources.

#### Example:
```hcl
provider "aws" {
  region = "us-east-1"
}
```
- **`region`**: Defines the AWS region for resource deployment.

### Resource Block
The `resource` block defines the actual infrastructure components.

#### Example:
```hcl
resource "aws_security_group" "web_sg" {
  name_prefix = "web-sg-"
  description = "Allow inbound HTTP and SSH traffic"
}
```
- **`name_prefix`**: Prefix for the Security Group name.
- **`ingress`/`egress`**: Rules for inbound and outbound traffic.

### Variable Block
The `variable` block is used to parameterize values, making the script reusable.

#### Example:
```hcl
variable "region" {
  default = "us-east-1"
}
```
- **`default`**: Specifies a default value.

### Data Block
The `data` block retrieves existing resources.

#### Example:
```hcl
data "aws_ami" "latest" {
  most_recent = true
  owners      = ["self"]
}
```
- **`most_recent`**: Fetches the latest AMI.

### Output Block
The `output` block displays resource attributes after execution.

#### Example:
```hcl
output "security_group_id" {
  value = aws_security_group.web_sg.id
}
```
- **`value`**: Specifies the attribute to output.

---

## 5. Applying the Script

### Steps:
1. Initialize Terraform:
   ```bash
   terraform init
   ```

2. Validate the script:
   ```bash
   terraform validate
   ```

3. Plan the execution:
   ```bash
   terraform plan
   ```

4. Apply the changes:
   ```bash
   terraform apply
   ```

5. Verify the Security Group in the AWS Console.

---

# Day-5: Terraform Modules and Dependencies

## What is a Terraform Module?
A **Terraform module** is a container for multiple resources that are used together. A module can include one or more `.tf` files in a directory and is a way to organize and reuse infrastructure configurations. Modules are particularly useful for creating reusable and standardized infrastructure components.

### Benefits of Using Modules
- **Reusability**: Define common configurations once and reuse them.
- **Maintainability**: Break down complex configurations into smaller, manageable pieces.
- **Scalability**: Easily replicate infrastructure for different environments (e.g., dev, staging, prod).
- **Collaboration**: Simplify collaboration by standardizing infrastructure code.

---

## Day:5 - Example Modules for VPC, Subnet, and EC2

### Module: VPC
#### `modules/vpc/main.tf`
```hcl
variable "vpc_cidr" {}
variable "tags" {}

resource "aws_vpc" "main" {
  cidr_block = var.vpc_cidr

  tags = merge({
    Name = "main-vpc"
  }, var.tags)
}

output "vpc_id" {
 value = aws_vpc.main.id
}
```

#### `modules/vpc/variables.tf`
```hcl
variable "vpc_cidr" {
 default = "10.0.0.0/16"
 description = "The CIDR block for the VPC."
}

variable "tags" {
 type        = map(string)
 default     = {}
 description = "Additional tags for the VPC."
}
```

### Module: Subnet
#### `modules/subnet/main.tf`
```hcl
variable "vpc_id" {}
variable "subnet_cidr" {}

resource "aws_subnet" "main" {
  vpc_id     = var.vpc_id
  cidr_block = var.subnet_cidr
  tags = {
    Name = "main-subnet"
  }
}

output "subnet_id" {
  value = aws_subnet.main.id
}
```

#### `modules/subnet/variables.tf`
```hcl
variable "vpc_id" {
  description = "The VPC ID where the subnet will be created."
}

variable "subnet_cidr" {
  default     = "10.0.1.0/24"
  description = "The CIDR block for the subnet."
}
```

### Module: EC2
#### `modules/ec2/main.tf`
```hcl
variable "subnet_id" {}
variable "ami" {}
variable "instance_type" {}

resource "aws_instance" "main" {
  ami           = var.ami
  instance_type = var.instance_type
  subnet_id     = var.subnet_id

  tags = {
    Name = "main-instance"
  }
}

output "instance_id" {
  value = aws_instance.main.id
}
```

#### `modules/ec2/variables.tf`
```hcl
variable "subnet_id" {
  description = "The Subnet ID where the instance will be launched."
}

variable "ami" {
  description = "The AMI ID to use for the instance."
}

variable "instance_type" {
  default     = "t2.micro"
  description = "The instance type for the EC2 instance."
}
```

---

## Types of Dependencies

### Implicit Dependencies
Terraform automatically handles resource dependencies based on references between resources.
Example:
```hcl
resource "aws_instance" "example" {
  subnet_id = aws_subnet.main.id  # Implicit dependency on the subnet
}
```

### Explicit Dependencies
Use `depends_on` to define dependencies explicitly.
Example:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
  depends_on    = [aws_vpc.main]  # Explicit dependency on the VPC
}
```

---

## Terraform Blocks Overview

### `terraform` Block
Defines Terraform settings, including backend configuration and required providers.
Example:
```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }

  backend "s3" {
    bucket = "my-terraform-state"
    key    = "state.tfstate"
    region = "us-west-2"
  }
}
```

### `module` Block
Used to call and configure a module.
Example:
```hcl
module "vpc" {
  source   = "./modules/vpc"
  vpc_cidr = "10.0.0.0/16"
  tags     = { Environment = "Dev" }
}
```

### `output` Block
Defines outputs that can be accessed after applying the configuration.
Example:
```hcl
output "vpc_id" {
  value = module.vpc.vpc_id
}
```

----

# Day-6 : Remote State Storage and Multi-Environment Setup

## Storing `terraform.tfstate` on a Remote Location

### Why Store State Remotely?
- Storing the state file remotely enables team collaboration.
- Provides state locking to prevent conflicts during simultaneous updates.
- Secures sensitive data stored in the state file.

### Example: AWS S3 Backend Configuration
Use the following configuration to store your Terraform state in an S3 bucket.

```hcl
terraform {
  backend "s3" {
    bucket         = "your-terraform-state-bucket"  # Replace with your bucket name
    key            = "environment-name/terraform.tfstate"  # Path within the bucket
    region         = "us-east-1"  # Replace with your AWS region
    encrypt        = true
    dynamodb_table = "terraform-lock-table"  # Optional for state locking
  }
}
```

### Steps to Configure Remote State
1. **Create an S3 Bucket**:
   - Use the AWS Management Console or CLI to create a bucket.

2. **Create a DynamoDB Table for State Locking (Optional)**:
   ```bash
   aws dynamodb create-table \
     --table-name terraform-lock-table \
     --attribute-definitions AttributeName=LockID,AttributeType=S \
     --key-schema AttributeName=LockID,KeyType=HASH \
     --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1
   ```

3. **Configure the Backend**:
   - Add the backend block in `main.tf`.

4. **Initialize the Backend**:
   ```bash
   terraform init
   ```

---

## Multi-Environment Script using `.tfvars`

### Why Use `.tfvars` Files?
- Manage environment-specific configurations (e.g., `dev`, `staging`, `prod`).
- Simplify Terraform commands and reduce errors.

### Folder Structure
Organize your project to keep environment-specific variables separate.

```
terraform-project/
├── dev/
│   ├── terraform.tfvars
├── staging/
│   ├── terraform.tfvars
├── prod/
│   ├── terraform.tfvars
├── main.tf
├── variables.tf
```

### Sample `main.tf`
Define the main configuration with variables for dynamic values.

```hcl
provider "aws" {
  region = var.aws_region
}

resource "aws_instance" "example" {
  ami           = var.ami
  instance_type = var.instance_type
  tags = {
    Name = var.env_name
  }
}
```

### Sample `variables.tf`
Define variables to parameterize the configuration.

```hcl
variable "aws_region" {
  description = "AWS region"
  type        = string
}

variable "ami" {
  description = "AMI ID to use for instances"
  type        = string
}

variable "instance_type" {
  description = "Type of EC2 instance"
  type        = string
}

variable "env_name" {
  description = "Environment name"
  type        = string
}
```

### Sample `.tfvars` File
Create a `.tfvars` file for each environment.

#### `dev/terraform.tfvars`
```hcl
aws_region    = "us-east-1"
ami           = "ami-0abcd1234abcd5678"
instance_type = "t2.micro"
env_name      = "development"
```

#### `staging/terraform.tfvars`
```hcl
aws_region    = "us-east-1"
ami           = "ami-0abcd1234abcd5678"
instance_type = "t2.small"
env_name      = "staging"
```

#### `prod/terraform.tfvars`
```hcl
aws_region    = "us-east-1"
ami           = "ami-0abcd1234abcd5678"
instance_type = "t3.medium"
env_name      = "production"
```

### Commands to Apply Configuration
Specify the environment when running Terraform commands:

#### For Development
```bash
terraform apply -var-file=dev/terraform.tfvars
```

#### For Staging
```bash
terraform apply -var-file=staging/terraform.tfvars
```

#### For Production
```bash
terraform apply -var-file=prod/terraform.tfvars
```

---

## Benefits of This Approach
- **Consistency**: Ensures configurations are environment-specific and not hardcoded.
- **Scalability**: Easily add new environments by creating additional `.tfvars` files.
- **Security**: Keeps sensitive data in a centralized backend with proper access controls.


------


# Terraform Workspace and Loops

## Concept of Terraform Workspace
Terraform workspaces are a way to manage multiple environments (e.g., development, staging, production) within a single Terraform configuration. Each workspace has its own state file, allowing separate management of infrastructure resources for different environments.

### Key Points about Workspaces:
- **Default Workspace**: Terraform starts with a `default` workspace.
- **Named Workspaces**: You can create additional workspaces with meaningful names for each environment.
- **State Isolation**: Each workspace maintains its own `terraform.tfstate` file.
- **Usage**: Workspaces are helpful for managing non-overlapping resources across environments.

### Commands to Manage Workspaces:
- **List Workspaces**:
  ```bash
  terraform workspace list
  ```
- **Create a Workspace**:
  ```bash
  terraform workspace new <workspace_name>
  ```
- **Switch to a Workspace**:
  ```bash
  terraform workspace select <workspace_name>
  ```
- **Delete a Workspace**:
  ```bash
  terraform workspace delete <workspace_name>
  ```

### Example:
```hcl
# Example of using workspace in a configuration
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "example" {
  bucket = "example-bucket-${terraform.workspace}"
  acl    = "private"
}
```
In this example, the S3 bucket name changes dynamically based on the active workspace.

---

## Terraform Loops
Terraform provides powerful constructs for iterating over collections like `list` and `map`. The primary looping mechanisms are `count`, `for_each`, and `for`.

### 1. `count`
- **Definition**: The `count` parameter allows you to specify how many instances of a resource to create.
- **Usage**: Works well for creating identical resources.

#### Example:
```hcl
resource "aws_instance" "example" {
  count         = 3
  ami           = "ami-12345678"
  instance_type = "t2.micro"
}
```
In this example, three EC2 instances are created.

#### Accessing Instances:
```hcl
aws_instance.example[0]  # First instance
aws_instance.example[1]  # Second instance
aws_instance.example[2]  # Third instance
```

### 2. `for_each`
- **Definition**: The `for_each` meta-argument allows iterating over `map` or `set` types to create resources with distinct properties.
- **Usage**: Useful when resource properties vary.

#### Example:
```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_s3_bucket" "example" {
  for_each = {
    dev  = "dev-bucket-unique-1"
    prod = "prod-bucket-unique-2"
  }

  bucket = each.value
}



```
This creates two S3 buckets: `dev-bucket` and `prod-bucket`.

#### Accessing Instances:
```hcl
aws_s3_bucket.example["dev"]  # Dev bucket
aws_s3_bucket.example["prod"] # Prod bucket
```

### 3. `for`
- **Definition**: The `for` expression is used to transform or filter collections.
- **Usage**: Commonly used in variables and outputs.

#### Example:
```hcl
variable "names" {
  default = ["Alice", "Bob", "Charlie"]
}

output "uppercase_names" {
  value = [for name in var.names : upper(name)]
}
```
This outputs the names in uppercase: `["ALICE", "BOB", "CHARLIE"]`.

#### Filtering with `for`:
```hcl
output "filtered_names" {
  value = [for name in var.names : name if length(name) > 3]
}
```
This filters names longer than three characters.

---

## Comparison Table
| Feature      | `count`                  | `for_each`                  | `for`                    |
|--------------|--------------------------|-----------------------------|--------------------------|
| Input Type   | Number                   | Map or Set                  | List, Map, or Set        |
| Use Case     | Create identical items   | Create unique items         | Transform or filter data |
| Example      | EC2 instances            | S3 buckets with unique IDs  | Modify list of names     |

---

# Terraform Commands and Provisioners

## Terraform Commands

### 1. **Taint Command**
The `taint` command marks a resource for recreation during the next `terraform apply`. This is useful when a specific resource needs to be replaced without altering the rest of the infrastructure.

#### **Syntax:**
```bash
terraform taint <resource_name>
```

#### **Example:**
```bash
terraform taint aws_instance.my_instance
```
This marks the `aws_instance.my_instance` resource for recreation.

---

### 2. **Import Command**
The `import` command allows importing existing infrastructure resources into Terraform state. This is helpful when managing resources created outside of Terraform.

#### **Syntax:**
```bash
terraform import <resource_type>.<resource_name> <resource_id>
```

#### **Example:**
```bash
terraform import aws_instance.my_instance i-0abcd1234efgh5678
```
This imports the AWS EC2 instance with ID `i-0abcd1234efgh5678` into Terraform as `aws_instance.my_instance`.

---

### 3. **Destroy Command**
The `destroy` command removes all resources defined in the configuration.

#### **Targeted Destroy (-t)**
You can destroy specific resources using the `-target` flag.

#### **Syntax:**
```bash
terraform destroy -target=<resource_type>.<resource_name>
```

#### **Example:**
```bash
terraform destroy -target=aws_instance.my_instance
```
This removes only the `aws_instance.my_instance` resource.

---

## Terraform Provisioners
Provisioners are used to execute scripts or configurations on a resource after it's created or before it's destroyed. 

### Types of Provisioners

#### 1. **Remote Provisioner**
Executes commands on a remote machine using SSH or WinRM.

#### **Syntax:**
```hcl
provisioner "remote-exec" {
  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("~/.ssh/id_rsa")
    host     = self.public_ip
  }

  inline = [
    "sudo apt update",
    "sudo apt install nginx -y"
  ]
}
```

#### **Use Case:**
- Installing software on a remote server after resource creation.

---

#### 2. **Local Provisioner**
Executes commands locally on the machine running Terraform.

#### **Syntax:**
```hcl
provisioner "local-exec" {
  command = "echo 'Deployment completed!' >> deployment.log"
}
```

#### **Use Case:**
- Writing logs or triggering local scripts after resource creation.

---

#### 3. **File Provisioner**
Copies files or directories from the machine running Terraform to the resource.

#### **Syntax:**
```hcl
provisioner "file" {
  source      = "./app/config.json"
  destination = "/tmp/config.json"

  connection {
    type     = "ssh"
    user     = "ec2-user"
    private_key = file("~/.ssh/id_rsa")
    host     = self.public_ip
  }
}
```

#### **Use Case:**
- Transferring configuration files or assets to remote servers.

---

## Summary Table
| Command/Provisioner | Purpose                              |
|---------------------|--------------------------------------|
| `terraform taint`   | Marks a resource for recreation      |
| `terraform import`  | Imports existing resources to state  |
| `terraform destroy` | Removes all or targeted resources    |
| `remote-exec`       | Runs commands on remote machines     |
| `local-exec`        | Executes local commands              |
| `file`              | Transfers files to remote resources  |

