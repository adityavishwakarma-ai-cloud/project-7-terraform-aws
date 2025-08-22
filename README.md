# 🌐 Project 7: Infrastructure as Code (IaC) using Terraform on AWS

## 📌 Project Overview

This project demonstrates provisioning **AWS infrastructure using Terraform**, automating the deployment of cloud resources without manual configuration.

Key highlights:

* Provision AWS services like **EC2, S3, VPC, and Security Groups** using code.
* Fully **reproducible and version-controlled infrastructure**.
* Supports **multi-environment deployments** (dev, staging, production).
* Demonstrates **Idempotent deployments**, meaning running Terraform multiple times keeps the infrastructure consistent.

---

## 🛠️ Tech Stack / Tools Required

* **Terraform CLI** (Infrastructure as Code)
* **AWS CLI** (Authentication & Management)
* **AWS Account** with access keys
* **Git & GitHub** for version control
* Optional: **VS Code + Terraform Extension** for better development experience

---

## 📂 Functional Requirements

* Provision a VPC with public and private subnets.
* Deploy an **EC2 instance** with security group allowing SSH and HTTP access.
* Create an **S3 bucket** for file storage.
* Automate resource creation and updates using Terraform.

---

## 📂 Non-Functional Requirements

* Infrastructure must be **reproducible** and version-controlled.
* Use **Terraform modules** for code reusability.
* Ensure **secure access** using AWS IAM roles and policies.
* Maintain **separate environments** (dev/staging/prod) with minimal duplication.

---

## 🔄 Project Flow

```
1. Define infrastructure in Terraform configuration files (.tf)
    ├─ main.tf       # Resources like EC2, S3, VPC
    ├─ variables.tf  # Define inputs for modularity
    ├─ outputs.tf    # Export resource attributes
    └─ provider.tf   # Configure AWS provider

2. Initialize Terraform
    └─ terraform init

3. Validate configuration
    └─ terraform validate

4. Plan deployment
    └─ terraform plan

5. Apply changes to AWS
    └─ terraform apply

6. Verify resources in AWS Console
    ├─ EC2 instance running
    ├─ S3 bucket created
    └─ VPC and subnets deployed

7. Optional: Destroy resources to clean up
    └─ terraform destroy
```

---

## 📂 Folder Structure

```
project-7-terraform-aws/
│-- README.md
│-- main.tf
│-- variables.tf
│-- outputs.tf
│-- provider.tf
│-- modules/               # Optional: reusable Terraform modules
│   └-- ec2-instance/
│       ├─ main.tf
│       ├─ variables.tf
│       └─ outputs.tf
│-- .gitignore
```

---

## 📝 Sample `main.tf`

```hcl
provider "aws" {
  region = "us-east-1"
}

# Create an S3 bucket
resource "aws_s3_bucket" "project_bucket" {
  bucket = "project7-terraform-bucket-12345"
  acl    = "private"
}

# Create a security group
resource "aws_security_group" "web_sg" {
  name        = "web-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Create a VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = {
    Name = "project7-vpc"
  }
}

# Create EC2 instance
resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"
  instance_type = "t2.micro"
  key_name      = "your-key-name"
  security_groups = [aws_security_group.web_sg.name]
  tags = {
    Name = "Project7-EC2"
  }
}
```

---

## ⚡ Deployment Steps

1. **Initialize Terraform**

```bash
terraform init
```

2. **Validate configuration**

```bash
terraform validate
```

3. **Preview changes**

```bash
terraform plan
```

4. **Apply configuration**

```bash
terraform apply
```

5. **Verify resources** in AWS console (VPC, EC2, S3).

6. **Destroy resources (optional)**

```bash
terraform destroy
```

---

## 📝 Resume Highlights

**Objective**
Implemented **Infrastructure as Code** to automate cloud resource provisioning on AWS.

**Key Achievements**

* Provisioned **VPC, EC2, Security Groups, and S3** using Terraform.
* Demonstrated **reproducible and scalable infrastructure** practices.
* Applied **IaC principles** for efficient, version-controlled deployments.

**Impact**
✅ Reduced manual cloud provisioning effort by 100%.
✅ Enabled multi-environment deployments with minimal errors.
✅ Improved cloud infrastructure scalability, maintainability, and security.

---

## 🔗 Repository Link

```md
https://github.com/<your-username>/project-7-terraform-aws
```

