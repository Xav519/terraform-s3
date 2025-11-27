# Terraform S3 Bucket Project

This project demonstrates how to **create and manage an AWS S3 bucket using Terraform**. It also covers **setting up Git and GitHub for version control**, and applying best practices for S3 configuration including **versioning, server-side encryption (SSE-S3), and public access restrictions**.

---
## 🏗️ Architecture Overview
![architectureS3TerraformSimple](https://github.com/user-attachments/assets/d264de74-2529-48e8-9833-148de3b3787e)

The diagram above illustrates the simple, sequential **Terraform workflow** used to provision the **Amazon S3 (Simple Storage Service) bucket** in this project.

The process involves three main stages:
* **Setup:** Installing Terraform, creating the project directory, and defining the resource in `main.tf`.
* **Execution:** Planning the configuration, ensuring **AWS Access Keys** are configured for authentication, and applying the changes using `terraform apply`.
* **Output:** The successful provisioning and launch of the secure **S3 bucket** in the AWS cloud.

---

## Table of Contents

1. [Prerequisites](#prerequisites)  
2. [Installing Terraform](#installing-terraform)  
3. [Setting up Git and GitHub](#setting-up-git-and-github)  
4. [Project Setup](#project-setup)  
5. [Terraform Configuration](#terraform-configuration)  
6. [Terraform Workflow](#terraform-workflow)  
7. [S3 Bucket Best Practices](#s3-bucket-best-practices)  
8. [Cleanup](#cleanup)  

---

## Prerequisites

- An **AWS account** with permissions to create S3 buckets.  
- **Terraform** installed.  
- **Git** installed.  
- A **GitHub account** for version control.  

---

## Installing Terraform

Terraform is a tool for managing infrastructure as code.

- **Windows:** Download the executable from [Terraform Downloads](https://www.terraform.io/downloads), unzip it, and add `terraform.exe` to a directory in your PATH (e.g., `C:\Windows\System32`).  
- **Verify installation:**

```bash
terraform -v
```

---

## Setting up Git and GitHub

1. Install Git and verify:

```bash
git --version
```

2. Initialize a Git repository:

```bash
git init
```

3. Link your local repository to GitHub:

```bash
git remote add origin https://github.com/<your-username>/<repo-name>.git
```

4. Commit and push changes:

```bash
git add .
git commit -m "Initial commit"
git push -u origin main
```

**Note:** If your GitHub repository already has files, you may need to pull first using `--allow-unrelated-histories` and resolve conflicts.

---

## Project Setup

1. Create a project folder:

```bash
mkdir AWS_projects/CreateS3BucketTerraform
cd AWS_projects/CreateS3BucketTerraform
```

2. Create Terraform configuration files:

```bash
touch main.tf
```

3. Initialize Terraform:

```bash
terraform init
```

---

## Terraform Configuration

This project creates a **private S3 bucket** with:

- **Versioning enabled**
- **Server-side encryption (SSE-S3)**
- **Public access blocked**
- **Tags** for identification

`main.tf` example:

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "my_bucket" {
  bucket = "xavierdupuis-terraform-bucket-2025"

  tags = {
    Name        = "MyBucket"
    Environment = "Dev"
  }
}

resource "aws_s3_bucket_public_access_block" "block_public" {
  bucket = aws_s3_bucket.my_bucket.id

  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

resource "aws_s3_bucket_server_side_encryption_configuration" "sse" {
  bucket = aws_s3_bucket.my_bucket.id

  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}

resource "aws_s3_bucket_versioning" "versioning" {
  bucket = aws_s3_bucket.my_bucket.id

  versioning_configuration {
    status = "Enabled"
  }
}

output "bucket_name" {
  value = aws_s3_bucket.my_bucket.bucket
}
```

---

## Terraform Workflow

1. **Plan your changes**:

```bash
terraform plan
```

2. **Apply changes**:

```bash
terraform apply
```

Confirm with `yes` when prompted.  

3. **Verify bucket creation**:

```bash
aws s3 ls
```

---

## S3 Bucket Best Practices Implemented

- **Versioning:** Retains all object versions for recovery.  
- **Server-side encryption (SSE-S3):** Automatically encrypts all objects at rest.  
- **Public access blocked:** Prevents unauthorized access.  
- **IAM-managed access:** No ACLs are used.  
- **Tagging:** Helps identify the environment and resource.

---

## Cleanup

To delete the bucket and all related resources safely:

```bash
terraform destroy
```

**Note:** Use `force_destroy = true` if deleting a bucket with existing objects and versions.

---

## References

- **HashiCorp.** Terraform Documentation. [https://developer.hashicorp.com/terraform/docs](https://developer.hashicorp.com/terraform/docs)  
- **Amazon Web Services.** AWS S3 Documentation. [https://docs.aws.amazon.com/s3/index.html](https://docs.aws.amazon.com/s3/index.html)  
- **Amazon Web Services.** AWS CLI Documentation. [https://docs.aws.amazon.com/cli/index.html](https://docs.aws.amazon.com/cli/index.html)  
- **OpenAI.** ChatGPT (GPT-4/5 Model). [https://chat.openai.com](https://chat.openai.com)  

*— Xavier Dupuis*
