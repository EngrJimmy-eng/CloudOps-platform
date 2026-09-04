CloudOps Platform

Infrastructure-as-Code (IaC) project for provisioning and managing AWS cloud infrastructure with Terraform.

The project demonstrates a modular approach to AWS infrastructure provisioning, including networking, security, IAM, EC2 compute, monitoring, and Amazon ECR resources.

Architecture

The infrastructure is designed around reusable Terraform modules:

                         AWS Cloud
                            │
                            ▼
                    ┌───────────────┐
                    │     VPC       │
                    │   Network     │
                    └───────┬───────┘
                            │
                ┌───────────┴───────────┐
                │                       │
        Public Subnet 1          Public Subnet 2
                │
                ▼
          ┌───────────┐
          │   EC2     │
          │  Server   │
          └─────┬─────┘
                │
       ┌────────┴────────┐
       │                 │
       ▼                 ▼
   IAM Role          CloudWatch
                         │
                  ┌──────┴──────┐
                  │             │
                Logs          Metrics
                  │             │
                  └──────┬──────┘
                         ▼
                       SNS
                       Alerts

                 Amazon ECR
                ┌─────────────┐
                │   Frontend  │
                │   Backend   │
                └─────────────┘

Key Features

- Infrastructure provisioned using Terraform
- Modular Terraform architecture
- AWS VPC networking
- Public subnets and Internet Gateway
- EC2 instance provisioning
- Security group configuration
- IAM roles and instance profiles
- Automatic Ubuntu AMI discovery
- CloudWatch monitoring configuration
- CloudWatch log groups and metrics
- SNS-based monitoring notifications
- Amazon ECR repositories for frontend and backend containers
- ECR scan-on-push enabled
- Environment validation for "dev", "staging", and "prod"
- Consistent AWS resource tagging
- Remote Terraform state using Amazon S3

Technology Stack

Technology| Purpose
Terraform| Infrastructure as Code
AWS| Cloud infrastructure
Amazon VPC| Networking
Amazon EC2| Compute
Amazon ECR| Container image registry
AWS IAM| Identity and access management
Amazon CloudWatch| Monitoring and logging
Amazon SNS| Alert notifications
Amazon S3| Terraform remote state
Ubuntu 24.04| EC2 operating system

Project Structure

CloudOps-platform/
│
├── terraform/
│   ├── backend.tf
│   ├── data.tf
│   ├── ecr.tf
│   ├── locals.tf
│   ├── main.tf
│   ├── outputs.tf
│   ├── provider.tf
│   ├── variables.tf
│   ├── versions.tf
│   │
│   ├── modules/
│   │   ├── network/
│   │   ├── security/
│   │   ├── iam/
│   │   ├── ec2/
│   │   └── monitoring/
│   │
│   └── scripts/
│       └── user_data.sh.tpl
│
└── README.md

Terraform Modules

Network

The network module is responsible for the core VPC infrastructure, including:

- VPC
- Public subnets
- Internet Gateway
- Route table
- Availability-zone configuration

Security

The security module manages security-related infrastructure such as EC2 security groups.

IAM

The IAM module provisions the IAM role and instance profile required by the EC2 instance.

EC2

The EC2 module provisions the application server using an automatically discovered Ubuntu 24.04 AMI.

The instance receives:

- Configurable instance type
- Public IP
- Security group
- IAM instance profile
- Bootstrap configuration through Terraform "user_data"

Monitoring

The monitoring module configures CloudWatch monitoring for the EC2 instance.

It supports:

- CloudWatch metrics
- CloudWatch Agent configuration
- CloudWatch log groups
- Systems Manager Parameter Store configuration
- SNS alert notifications

Container Registry

For production environments, the project provisions separate Amazon ECR repositories for:

- Frontend
- Backend

ECR repositories have image scanning enabled on image push.

The repositories are created conditionally when:

environment = "prod"

Environments

The Terraform configuration supports three environments:

dev
staging
prod

Terraform validates the environment value to prevent unsupported environment names.

Example:

environment = "prod"

Resource Tagging

Resources receive consistent tags through the AWS provider's "default_tags" configuration.

Example tags include:

Project
Environment
ManagedBy
Repository
Owner

This makes resources easier to identify, manage, and audit within AWS.

Terraform Backend

Terraform state is configured to use an Amazon S3 backend:

terraform {
  backend "s3" {}
}

Using remote state helps keep Terraform state outside the local development environment and supports infrastructure management from different environments.

Prerequisites

Before deploying the infrastructure, install/configure:

- Terraform >= 1.5.0
- AWS CLI
- An AWS account
- AWS credentials with appropriate permissions
- An S3 bucket for Terraform state

The project uses the AWS provider:

hashicorp/aws ~> 6.0

Configuration

Create a Terraform variables file:

cd terraform
nano terraform.tfvars

Example:

project_name = "cloudops"
environment  = "dev"
server_name  = "app-server"
region       = "eu-west-1"
instance_type = "t3.micro"
alert_email  = "your-email@example.com"

Use your own values for the variables.

Do not commit credentials, passwords, private keys, or sensitive Terraform variable files to GitHub.

Initialize Terraform

terraform init

If using the S3 backend, configure the backend according to your AWS environment.

Validate the Configuration

terraform validate

Format Terraform Code

terraform fmt -recursive

Review the Execution Plan

terraform plan

Review the resources Terraform intends to create before applying the configuration.

Deploy

terraform apply

Confirm the deployment when Terraform asks for approval.

View Outputs

After deployment:

terraform output

Individual outputs can also be queried:

terraform output ec2_instance_id
terraform output ec2_public_ip
terraform output vpc_id

Destroy Infrastructure

When the environment is no longer required:

terraform destroy

Review the resources carefully before confirming destruction.

Security Considerations

This project incorporates several infrastructure security practices:

- IAM roles instead of embedding AWS credentials in EC2 configuration
- Security groups for network access control
- ECR image scanning on push
- Environment validation
- Consistent resource tagging
- Remote Terraform state
- Separation of infrastructure concerns through modules
- No credentials stored directly in Terraform source code

Secrets and sensitive configuration should be supplied through secure mechanisms rather than committed to the repository.

Outputs

The Terraform configuration exposes useful infrastructure information including:

- VPC ID
- Public subnet IDs
- Internet Gateway ID
- Route table ID
- EC2 instance ID
- EC2 public IP
- EC2 public DNS
- Security group ID
- IAM role name
- Instance profile name
- SNS topic ARN
- CloudWatch log group
- CloudWatch Agent parameter

DevOps Skills Demonstrated

This project demonstrates practical experience with:

- Infrastructure as Code
- Terraform
- AWS cloud infrastructure
- Modular infrastructure design
- EC2 provisioning
- VPC networking
- IAM
- CloudWatch monitoring
- SNS notifications
- Container registries
- Infrastructure security
- Environment-based configuration
- Remote Terraform state
- Automated infrastructure provisioning

Project Purpose

CloudOps-platform was created as a practical cloud engineering and DevOps project to demonstrate how AWS infrastructure can be provisioned, configured, monitored, and managed using Terraform rather than manually creating resources through the AWS Console.

The project emphasizes repeatability, modularity, security, and infrastructure automation.

Author

Ikenna Ndumele

Cloud / DevOps Engineer
Full-Stack Developer

GitHub: "EngrJimmy-eng"
