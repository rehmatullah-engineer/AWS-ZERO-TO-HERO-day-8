import pypandoc

content = r'''<div align="center">

# ☁️ AWS & AWS CLI — Complete Notes & Cheat Sheet

### Learn • Build • Deploy • Manage • Grow

A practical, beginner-friendly reference for AWS fundamentals, IAM, AWS CLI commands, and core cloud services.

<p>
  <img src="https://img.shields.io/badge/AWS-Cloud%20Computing-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white" alt="AWS Cloud Computing">
  <img src="https://img.shields.io/badge/AWS%20CLI-Command%20Line-232F3E?style=for-the-badge&logo=gnubash&logoColor=white" alt="AWS CLI">
  <img src="https://img.shields.io/badge/Level-Beginner%20to%20Intermediate-16A34A?style=for-the-badge" alt="Beginner to Intermediate">
</p>

**A quick-reference guide for learning AWS concepts and practicing common CLI workflows.**

</div>

---

## 📚 Table of Contents

- [About This Repository](#-about-this-repository)
- [What Is AWS?](#-what-is-aws)
- [AWS Global Infrastructure](#-aws-global-infrastructure)
- [IAM — Identity and Access Management](#-iam--identity-and-access-management)
- [AWS CLI Introduction](#-aws-cli-introduction)
- [Install and Configure AWS CLI](#-install-and-configure-aws-cli)
- [Core AWS CLI Commands](#-core-aws-cli-commands)
- [Amazon EC2](#-amazon-ec2)
- [Amazon S3](#-amazon-s3)
- [Amazon VPC](#-amazon-vpc)
- [Amazon RDS](#-amazon-rds)
- [AWS Lambda](#-aws-lambda)
- [Other Useful AWS Services](#-other-useful-aws-services)
- [Global CLI Options](#-global-cli-options)
- [Hands-on Mini Project](#-hands-on-mini-project)
- [Security Best Practices](#-security-best-practices)
- [Troubleshooting](#-troubleshooting)
- [Official Resources](#-official-resources)
- [Learning Roadmap](#-learning-roadmap)

---

## 🎯 About This Repository

This repository organizes essential AWS and AWS CLI notes into one convenient reference. It is designed for students, cloud beginners, developers, and aspiring DevOps engineers who want to understand cloud concepts and practice common commands.

### What you'll learn

- Understand cloud computing and AWS global infrastructure.
- Learn IAM users, groups, roles, and policies.
- Install and configure AWS CLI.
- Work with core services such as EC2, S3, VPC, RDS, and Lambda.
- Read common AWS errors and troubleshoot permissions.
- Follow security-first practices while working in the cloud.

> **Important:** Commands are examples, not a guarantee that an operation will succeed in every account. Your IAM permissions, AWS Region, resource names, quotas, and configuration affect the result. Review commands before running them, especially commands that create, modify, or delete resources.

---

## ☁️ What Is AWS?

**Amazon Web Services (AWS)** is a cloud computing platform that provides on-demand compute, storage, databases, networking, security, analytics, and many other services.

### Key benefits

| Benefit | What it means |
|---|---|
| Scalability | Increase or reduce resources as demand changes. |
| Pay-as-you-go | Pay for many services based on usage; check service-specific pricing. |
| Global infrastructure | Deploy workloads in supported AWS Regions around the world. |
| Managed services | AWS operates much of the underlying infrastructure for managed services. |
| Security tools | Use IAM, encryption, logging, network controls, and monitoring. |
| Automation | Manage resources using the CLI, SDKs, APIs, and infrastructure as code. |

### Cloud service models

- **IaaS:** Infrastructure building blocks, such as virtual machines and networks.
- **PaaS:** Platforms that simplify application deployment and management.
- **SaaS:** Complete software applications delivered over the internet.

---

## 🌍 AWS Global Infrastructure

| Component | Meaning |
|---|---|
| **Region** | A separate geographic area containing AWS infrastructure. |
| **Availability Zone (AZ)** | An isolated location within a Region, designed to help build resilient applications. |
| **Edge location** | A point of presence used by services such as Amazon CloudFront to deliver content closer to users. |

### Example

A workload in `us-east-1` can use multiple Availability Zones in that Region to improve resilience. The exact AZs and available services depend on the Region and account.

**Remember:** A Region is not the same as an Availability Zone. Many AWS resources are regional, while some settings and services are global or have other scopes.

---

## 🔐 IAM — Identity and Access Management

IAM controls **who can access which AWS resources and what actions they can perform**.

| IAM concept | Purpose |
|---|---|
| **User** | An identity that can be assigned permissions. Prefer federation or IAM Identity Center for human access where appropriate. |
| **Group** | A way to assign permissions to a collection of IAM users. |
| **Role** | An identity with permissions that can be assumed by trusted users, services, or accounts. |
| **Policy** | A JSON document defining allowed or denied actions and resources. |

### Least-privilege principle

Grant only the permissions required to complete a task. Avoid using administrator access for everyday work.

### Inspect the current identity

```bash
aws sts get-caller-identity
```

This returns the account, ARN, and user or role identity associated with the active credentials.

### IAM policy reminders

- `Action` defines API operations.
- `Resource` identifies the resources the policy applies to, where supported.
- `Effect` is usually `Allow` or `Deny`.
- `Condition` can add constraints to a policy.
- An explicit deny overrides an allow.
- Prefer temporary credentials and IAM roles over long-lived access keys.

> **Security warning:** Never commit access keys, secret keys, session tokens, or credential files to GitHub. If credentials are exposed, revoke or rotate them immediately and review CloudTrail for unexpected activity.

---

## 💻 AWS CLI Introduction

The **AWS Command Line Interface (AWS CLI)** lets you manage AWS services from a terminal, script, or automation workflow.

### Why use the CLI?

- Automate repeatable tasks.
- Manage services without manually navigating the console.
- Build scripts and deployment workflows.
- Query resources and format returned data.
- Integrate AWS operations into CI/CD pipelines.

### General command structure

```bash
aws <service> <operation> [options]
```

Example:

```bash
aws s3 ls
```

This attempts to list buckets visible to the active identity. It requires the relevant permissions.

---

## ⚙️ Install and Configure AWS CLI

Install the AWS CLI using the official instructions for your operating system:

**Official guide:** https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html

### Verify installation

```bash
aws --version
```

### Configure a profile

```bash
aws configure
```

The CLI prompts for:

- AWS Access Key ID
- AWS Secret Access Key
- Default Region name, for example `us-east-1`
- Default output format, for example `json`

Use credentials issued for your account or training environment. For production and supported organization setups, prefer AWS IAM Identity Center, role-based access, or another approved temporary-credential method.

### Configure a named profile

```bash
aws configure --profile dev
```

Use it with a command:

```bash
aws sts get-caller-identity --profile dev
```

### Configuration locations

| Operating system | Common location |
|---|---|
| Linux / macOS | `~/.aws/credentials` and `~/.aws/config` |
| Windows | `%UserProfile%\.aws\credentials` and `%UserProfile%\.aws\config` |

Keep these files private. Do not upload them to a repository or share them in screenshots.

---

## 🧰 Core AWS CLI Commands

| Task | Command |
|---|---|
| Show CLI version | `aws --version` |
| Show help | `aws help` |
| Show service help | `aws s3 help` |
| Check current identity | `aws sts get-caller-identity` |
| List configured profiles | `aws configure list-profiles` |
| View active configuration | `aws configure list` |
| List available Regions for EC2 | `aws ec2 describe-regions` |
| Use a specific Region | `--region us-east-1` |
| Use a named profile | `--profile dev` |
| Return JSON output | `--output json` |
| Return table output | `--output table` |
| Filter results with JMESPath | `--query 'Reservations[].Instances[].InstanceId'` |

**Tip:** A command can use the wrong profile or Region and appear to show missing resources. Confirm your identity, profile, and Region before troubleshooting.

---

## 🖥️ Amazon EC2

**Amazon Elastic Compute Cloud (EC2)** provides virtual servers called instances.

### Key terms

- **AMI:** Amazon Machine Image used to launch an instance.
- **Instance type:** Defines compute, memory, and networking capacity.
- **Key pair:** Used for supported login methods; keep the private key secure.
- **Security group:** Stateful virtual firewall for instance traffic.
- **EBS volume:** Block storage that can be attached to an instance.
- **Elastic IP:** Static public IPv4 address, subject to AWS pricing and allocation rules.
- **Launch template:** Reusable instance launch configuration.

### Useful commands

```bash
# Describe EC2 instances in the selected Region
aws ec2 describe-instances

# List instance IDs and states
aws ec2 describe-instances \
  --query 'Reservations[].Instances[].[InstanceId,State.Name]' \
  --output table

# Start an existing instance
aws ec2 start-instances --instance-ids i-0123456789abcdef0

# Stop an existing instance
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
```

Replace the example instance ID with an instance you own or are authorized to manage.

> **Cost note:** Stopping an instance generally stops its instance compute charges, but attached storage and other resources may still incur charges. Review the pricing rules for your setup.

---

## 🪣 Amazon S3

**Amazon Simple Storage Service (S3)** is object storage for files, backups, logs, static assets, and data lakes.

### Key concepts

- **Bucket:** Container for objects.
- **Object:** A file and its associated metadata.
- **Key:** The object's name/path within a bucket.
- **Versioning:** Keeps multiple versions of objects when enabled.
- **Lifecycle rules:** Transition or expire objects according to configured rules.
- **Encryption:** Protect data at rest using supported encryption options.
- **Bucket policy:** Resource-based policy that controls access to a bucket and its objects.

### Useful commands

```bash
# List buckets; requires s3:ListAllMyBuckets
aws s3 ls

# List objects in a bucket
aws s3 ls s3://YOUR-BUCKET-NAME/

# Upload a file
aws s3 cp index.html s3://YOUR-BUCKET-NAME/

# Download a file
aws s3 cp s3://YOUR-BUCKET-NAME/index.html ./index.html

# Copy a local directory recursively
aws s3 cp ./website s3://YOUR-BUCKET-NAME/ --recursive

# Sync a local directory with a bucket
aws s3 sync ./website s3://YOUR-BUCKET-NAME/
```

Replace `YOUR-BUCKET-NAME` with your bucket name. Ensure the bucket exists and your identity has the required permissions.

**Security tip:** Keep public access blocked unless public access is explicitly required and approved. Use IAM policies, bucket policies, and encryption deliberately.

---

## 🌐 Amazon VPC

**Amazon Virtual Private Cloud (VPC)** lets you define an isolated virtual network for AWS resources.

### Core components

- **VPC:** Your logical network.
- **Subnet:** A range of IP addresses within a VPC, associated with an Availability Zone.
- **Route table:** Determines where network traffic is directed.
- **Internet gateway:** Enables internet connectivity for appropriately routed resources.
- **NAT gateway:** Allows supported outbound connectivity from private subnets; it does not by itself allow unsolicited inbound connections.
- **Security group:** Stateful traffic control associated with supported resources.
- **Network ACL:** Stateless traffic rules applied at the subnet level.

### Networking checklist

1. Choose a non-overlapping CIDR range.
2. Create subnets in the required Availability Zones.
3. Associate the correct route tables.
4. Configure internet or NAT connectivity only where needed.
5. Restrict inbound and outbound rules to required traffic.
6. Test connectivity and review logs when troubleshooting.

---

## 🗄️ Amazon RDS

**Amazon Relational Database Service (RDS)** helps you run and manage supported relational database engines.

### Common engines

MySQL, PostgreSQL, MariaDB, Oracle, Microsoft SQL Server, and Amazon Aurora (availability and features vary by engine and Region).

### Useful commands

```bash
# Describe database instances
aws rds describe-db-instances

# Describe a specific DB instance
aws rds describe-db-instances \
  --db-instance-identifier YOUR-DB-IDENTIFIER
```

Database creation and deletion commands can incur costs or permanently remove resources. Before running them, confirm the identifier, backups, retention requirements, network access, and account permissions.

### RDS best practices

- Keep databases private unless a public endpoint is specifically necessary.
- Restrict access with security groups.
- Use strong credentials and secrets-management practices.
- Enable backups and appropriate monitoring.
- Encrypt data where required.
- Plan for Multi-AZ or other resilience options based on the workload.

---

## ⚡ AWS Lambda

**AWS Lambda** runs code in response to events without requiring you to manage the underlying servers directly.

### Common use cases

- APIs and backend functions
- File-processing workflows
- Scheduled jobs
- Event-driven automation
- Integrations with services such as S3, EventBridge, and SQS

### Useful commands

```bash
# List Lambda functions
aws lambda list-functions

# Invoke a function synchronously
aws lambda invoke \
  --function-name YOUR-FUNCTION-NAME \
  --payload '{}' \
  response.json
```

The required payload format can depend on the CLI version and function configuration. Check the current AWS CLI documentation for binary-format and invocation options.

---

## 🧩 Other Useful AWS Services

| Service | Main purpose |
|---|---|
| **Amazon CloudFront** | Content delivery network (CDN). |
| **Amazon Route 53** | DNS and domain-routing services. |
| **Elastic Load Balancing (ELB)** | Distributes traffic across targets. |
| **EC2 Auto Scaling** | Adjusts EC2 capacity based on policies and demand. |
| **Amazon ECR** | Stores container images. |
| **Amazon ECS / EKS** | Runs and orchestrates containerized workloads. |
| **Amazon DynamoDB** | Managed NoSQL database. |
| **Amazon SQS** | Message queuing. |
| **Amazon SNS** | Pub/sub messaging and notifications. |
| **Amazon CloudWatch** | Metrics, logs, alarms, and monitoring. |
| **AWS CloudTrail** | Records account and API activity for auditing. |
| **AWS Secrets Manager** | Stores and manages secrets. |

---

## 🧭 Useful AWS CLI Global Options

| Option | Purpose |
|---|---|
| `--region us-east-1` | Select a Region for a command. |
| `--profile dev` | Use a named profile. |
| `--output json` | Format output as JSON. |
| `--output table` | Format output as a table. |
| `--query '...'` | Filter output with JMESPath. |
| `--no-cli-pager` | Disable the CLI output pager. |
| `--debug` | Print detailed diagnostic information. Use carefully; logs may contain sensitive details. |

### Example

```bash
aws ec2 describe-instances \
  --region us-east-1 \
  --profile dev \
  --output table
```

---

## 🧪 Hands-on Mini Project

This beginner project demonstrates a simple S3 upload workflow. It assumes you already have an AWS CLI profile and permission to create and use an S3 bucket.

### 1. Verify your identity

```bash
aws sts get-caller-identity
```

### 2. Create a globally unique bucket name

Choose a unique name and follow current S3 bucket naming requirements. For Regions other than `us-east-1`, include the appropriate location constraint.

```bash
aws s3 mb s3://YOUR-UNIQUE-BUCKET-NAME --region us-east-1
```

### 3. Create a sample file

```bash
echo "Hello from AWS CLI" > index.html
```

### 4. Upload the file

```bash
aws s3 cp index.html s3://YOUR-UNIQUE-BUCKET-NAME/
```

### 5. Verify the upload

```bash
aws s3 ls s3://YOUR-UNIQUE-BUCKET-NAME/
```

### 6. Clean up when finished

```bash
aws s3 rm s3://YOUR-UNIQUE-BUCKET-NAME/index.html
aws s3 rb s3://YOUR-UNIQUE-BUCKET-NAME
```

Only remove resources created for this exercise and confirm the bucket is empty before deleting it. Do not run cleanup commands against production resources.

---

## 🛡️ Security Best Practices

- [ ] Follow least privilege for users, roles, and policies.
- [ ] Prefer temporary credentials and role-based access.
- [ ] Enable MFA for privileged human access.
- [ ] Never commit credentials to source control.
- [ ] Restrict security groups and network ACLs to required traffic.
- [ ] Keep S3 Block Public Access enabled unless public access is required.
- [ ] Encrypt data in transit and at rest where appropriate.
- [ ] Enable and review CloudTrail logging.
- [ ] Use CloudWatch alarms for important operational signals.
- [ ] Review AWS Budgets and billing alerts to monitor spending.
- [ ] Delete unused resources after labs.
- [ ] Rotate or revoke exposed credentials immediately.

---

## 🧯 Troubleshooting

| Error / symptom | Common cause | What to check |
|---|---|---|
| `AccessDenied` | Missing permission, explicit deny, or resource policy restriction | Active identity, IAM policies, bucket/resource policies, permission boundaries, and organization controls. |
| `InvalidClientTokenId` | Invalid or unrecognized credentials | Profile, access key, expired or malformed credentials, and credential source. |
| `ExpiredToken` | Temporary credentials have expired | Refresh credentials or re-authenticate through the approved identity flow. |
| Resource not found | Wrong Region, identifier, or account/profile | Run `aws sts get-caller-identity`; verify the Region and exact resource ID. |
| CLI command not found | CLI not installed or not on `PATH` | Reinstall or update the PATH, then run `aws --version`. |
| Connection timeout | Network, endpoint, proxy, or security configuration | Check connectivity, endpoints, DNS, routing, and relevant firewall rules. |

### Helpful debug commands

```bash
aws sts get-caller-identity
aws configure list
aws configure list-profiles
aws ec2 describe-regions --output table
```

Use `--debug` only when necessary, and redact tokens, account details, and other sensitive information before sharing logs.

---

## 🔗 Official Resources

- [AWS Management Console](https://console.aws.amazon.com/)
- [AWS Free Tier](https://aws.amazon.com/free/)
- [AWS Documentation](https://docs.aws.amazon.com/)
- [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/)
- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/)
- [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)
- [Amazon EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [Amazon S3 Documentation](https://docs.aws.amazon.com/s3/)
- [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/)
- [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/)
- [AWS Lambda Documentation](https://docs.aws.amazon.com/lambda/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

---

## 🗺️ Suggested Learning Roadmap

1. **Cloud fundamentals** — Regions, Availability Zones, shared responsibility, and pricing.
2. **IAM and security** — Users, roles, policies, MFA, and least privilege.
3. **AWS CLI** — Profiles, Regions, output formats, queries, and troubleshooting.
4. **Storage and compute** — S3, EC2, EBS, and AMIs.
5. **Networking** — VPCs, subnets, route tables, gateways, and security groups.
6. **Databases and serverless** — RDS, DynamoDB, and Lambda.
7. **Operations** — CloudWatch, CloudTrail, budgets, and alarms.
8. **Projects** — Build, document, secure, and clean up a small cloud deployment.

---

<div align="center">

### 🚀 Learn AWS by building real projects.

**Understand the concept → Practice the command → Verify the result → Clean up safely**

*Cloud skills are built one hands-on lab at a time.*

</div>

