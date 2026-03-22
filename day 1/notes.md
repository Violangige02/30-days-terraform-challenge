# Day 1 — Introduction to Terraform and Infrastructure as Code

## Overview
Today was about foundations. Understanding why Terraform exists, what
problem it solves, and how it fits into the modern infrastructure
landscape. Also completed full environment setup.

---

## Environment Setup

### Terraform Installation
Downloaded the Windows binary from the official HashiCorp site,
extracted and moved `terraform.exe` to `C:\Windows\System32\`.

```bash
terraform version
```
```
Terraform v1.14.7
on windows_386
```

### AWS CLI Verification
Already configured from previous AWS projects.

```bash
aws sts get-caller-identity
```
```json
{
    "UserId": "AIDA***********",
    "Account": "************",
    "Arn": "arn:aws:iam::************:user/Vee"
}
```

### Tools Installed
| Tool | Version | Status |
|---|---|---|
| Terraform | v1.14.7 | ✅ Installed |
| AWS CLI | v2.x | ✅ Configured |
| VS Code | Latest | ✅ Installed |
| HashiCorp Terraform Extension | Latest | ✅ Installed |
| AWS Toolkit Extension | Latest | ✅ Installed |

---

## What is Infrastructure as Code?

Before IaC, infrastructure was created manually — clicking through
AWS console, running one-off commands, no record of what was done.

**Problems with manual infrastructure:**
- No record of what was created or why
- Cannot reproduce environments reliably
- Human error causes inconsistencies
- No way to review changes before applying
- Team members do things differently each time

I experienced this pain directly building AWS projects manually —
a multi-region Transit Gateway, a Serverless API with Lambda and RDS,
and a containerized portfolio on ECS Fargate. Each took hours of
clicking. Terraform solves all of this.

**IaC means:** Managing infrastructure through code files that can be
versioned, reviewed, shared and reused exactly like application code.

---

## The 4 Categories of IaC Tools

| Category | Examples | Purpose |
|---|---|---|
| Ad hoc scripts | Bash, PowerShell | Step by step commands |
| Configuration management | Ansible, Chef, Puppet | Configure existing servers |
| Server templating | Docker, Packer | Create server images |
| Provisioning tools | Terraform, CloudFormation | Create infrastructure |

**Terraform is a provisioning tool** — it creates the actual
infrastructure: VPCs, databases, load balancers, servers.

---

## Declarative vs Imperative

**Imperative** — you describe the steps:
```
1. Create a VPC
2. Create a subnet in that VPC
3. Create an internet gateway
4. Attach the gateway to the VPC
```

**Declarative** — you describe the end state:
```hcl
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
}

resource "aws_subnet" "main" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
}
```

Terraform is **declarative**. You describe what should exist and
Terraform figures out how to make it happen. If it already exists,
Terraform does nothing. If it changed, Terraform updates only what
changed.

---

## Why Terraform Over CloudFormation?

| Feature | Terraform | CloudFormation |
|---|---|---|
| Cloud support | AWS, Azure, GCP, 3000+ providers | AWS only |
| Language | HCL — clean and readable | JSON/YAML — verbose |
| State management | terraform.tfstate file | Managed by AWS |
| Community | Massive open source ecosystem | AWS only |
| Portability | Works on any cloud | AWS locked in |

Companies use multiple clouds and services — Terraform is the
industry standard because one tool covers everything.

---

## How Terraform Works

### Core Concepts

**Provider**
A plugin that lets Terraform talk to a specific platform.
```hcl
provider "aws" {
  region = "eu-west-2"
}
```

**Resource**
The actual infrastructure you want to create.
```hcl
resource "aws_vpc" "my_vpc" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "my-vpc"
  }
}
```

**State File**
Terraform keeps a `terraform.tfstate` file tracking everything it
has created. This is Terraform's memory — it knows what exists,
what changed, and what needs updating.

> ⚠️ Never delete the state file. Never commit it to GitHub with
> sensitive values. Use remote state in S3 for team projects.

### The Terraform Workflow

```
Write .tf files
      ↓
terraform init
Downloads providers, sets up working directory
      ↓
terraform plan
Shows exactly what will change — review carefully
      ↓
terraform apply
Executes the changes — type yes to confirm
      ↓
terraform destroy
Deletes everything Terraform created
```

**Rule: Plan before apply, always.**
Seeing exactly what will change before it happens is what makes
Terraform safe. Never run apply without reviewing the plan first.

---

## Key Terms

| Term | Definition |
|---|---|
| HCL | HashiCorp Configuration Language — the language Terraform uses |
| Provider | Plugin for a platform like AWS or Azure |
| Resource | A piece of infrastructure like a VPC or EC2 instance |
| State | Terraform's record of what it has created |
| Module | Reusable package of Terraform code — like a function |
| Plan | Preview of changes before applying |
| Apply | Execute the changes |
| Destroy | Delete everything Terraform created |

---

## Lab Takeaways — Benefits of IaC

The lab demonstrated how infrastructure defined as code can be:
- **Versioned** — stored in Git, every change tracked
- **Reviewed** — team members review before applying
- **Reproduced** — identical environments every time
- **Automated** — no manual steps, no human error

The biggest thing that clicked was the **state file concept**.
Terraform tracks what it created so on the next run it knows
exactly what to add, change or remove. Reading about this did
not make it real — running the commands and watching the state
file update made it concrete.

---

## Why I Am Doing This Challenge

I have built AWS projects manually:
- ✅ Multi-region VPC networking with Transit Gateway
- ✅ Serverless Notes API — Lambda + API Gateway + RDS MySQL
- ✅ Containerized portfolio on ECS Fargate with Load Balancer

Every project took hours of clicking through AWS console. Terraform
means I can recreate any of those environments in minutes with one
command. My goal for this 30-day challenge is to write
production-grade Terraform and add it to my cloud portfolio as I
work toward becoming a DevOps engineer.

---



---

## Repository Structure
```
30-days-terraform-challenge/
├── README.md
├── day1/
│   └── notes.md          ← this file
├── day2/
│   └── main.tf
├── day3/
│   └── main.tf
...
```

---

*Part of the 30 Days Terraform Challenge by AWS AI/ML UserGroup Kenya,
Meru HashiCorp User Group, and EveOps.*
