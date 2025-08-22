## 1️⃣ Folder Structure

```
project-7-terraform-aws/
│-- README.md
│-- provider.tf          # AWS provider configuration
│-- main.tf              # Resources (VPC, EC2, S3, Security Group)
│-- variables.tf         # Input variables
│-- outputs.tf           # Output resource attributes
│-- modules/             # Optional: reusable modules
│   └-- ec2-instance/
│       ├─ main.tf
│       ├─ variables.tf
│       └─ outputs.tf
│-- .gitignore
```

---

## 2️⃣ Provider Configuration – `provider.tf`

```hcl
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  required_version = ">= 1.4.0"
}

provider "aws" {
  region = var.aws_region
}
```

---

## 3️⃣ Variables – `variables.tf`

```hcl
variable "aws_region" {
  description = "AWS Region to deploy resources"
  type        = string
  default     = "us-east-1"
}

variable "ec2_instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "ec2_key_name" {
  description = "Key pair name for SSH access"
  type        = string
}

variable "s3_bucket_name" {
  description = "Name of S3 bucket"
  type        = string
  default     = "project7-terraform-bucket-12345"
}
```

---

## 4️⃣ Main Resources – `main.tf`

```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  tags = { Name = "project7-vpc" }
}

# Security Group
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

# EC2 Instance
resource "aws_instance" "web" {
  ami           = "ami-0c02fb55956c7d316"  # Amazon Linux 2
  instance_type = var.ec2_instance_type
  key_name      = var.ec2_key_name
  security_groups = [aws_security_group.web_sg.name]

  tags = { Name = "Project7-EC2" }
}

# S3 Bucket
resource "aws_s3_bucket" "project_bucket" {
  bucket = var.s3_bucket_name
  acl    = "private"
}
```

---

## 5️⃣ Outputs – `outputs.tf`

```hcl
output "ec2_instance_id" {
  description = "EC2 Instance ID"
  value       = aws_instance.web.id
}

output "ec2_public_ip" {
  description = "Public IP of EC2"
  value       = aws_instance.web.public_ip
}

output "s3_bucket_name" {
  description = "Name of S3 bucket"
  value       = aws_s3_bucket.project_bucket.id
}
```

---

## 6️⃣ Optional EC2 Module – `modules/ec2-instance/`

### a) `main.tf`

```hcl
resource "aws_instance" "web" {
  ami           = var.ami
  instance_type = var.instance_type
  key_name      = var.key_name
  vpc_security_group_ids = [var.security_group_id]

  tags = { Name = var.instance_name }
}
```

### b) `variables.tf`

```hcl
variable "ami" {}
variable "instance_type" {}
variable "key_name" {}
variable "security_group_id" {}
variable "instance_name" {}
```

### c) `outputs.tf`

```hcl
output "instance_id" {
  value = aws_instance.web.id
}
output "instance_ip" {
  value = aws_instance.web.public_ip
}
```

---

## 7️⃣ Deployment Steps

1. **Initialize Terraform**

```bash
terraform init
```

2. **Validate Configuration**

```bash
terraform validate
```

3. **Preview Plan**

```bash
terraform plan -var="ec2_key_name=<your-key-pair>"
```

4. **Apply Configuration**

```bash
terraform apply -var="ec2_key_name=<your-key-pair>"
```

5. **Verify Resources** in AWS Console:

* EC2 instance running
* Security group attached
* S3 bucket created

6. **Destroy Resources (Optional)**

```bash
terraform destroy -var="ec2_key_name=<your-key-pair>"
```


