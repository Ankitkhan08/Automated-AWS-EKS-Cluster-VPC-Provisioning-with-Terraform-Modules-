# Automated AWS EKS Cluster & VPC Provisioning with Terraform

Provisioned a production-ready Amazon EKS cluster and full network 
infrastructure on AWS using modular Terraform — no manual console 
steps required.

## What this does
- Creates a new VPC with public/private subnets across multiple 
  availability zones
- Provisions NAT Gateways, route tables, and internet gateway
- Deploys a fully configured EKS cluster using the official 
  terraform-aws-modules/eks module
- Configures autoscaling node groups using Spot Instances for 
  cost efficiency
- Entire stack is reproducible from a single `terraform apply`

## Architecture
```
Developer → terraform apply
                │
                ├── VPC (vpc.tf)
                │     ├── Public Subnets (2 AZs)
                │     ├── Private Subnets (2 AZs)
                │     ├── NAT Gateway
                │     └── Route Tables
                │
                └── EKS Cluster (eks.tf)
                      ├── Managed Node Group
                      ├── Spot Instances (cost-optimized)
                      └── Autoscaling Config
```

## Tech Stack
- **IaC:** Terraform
- **Cloud:** AWS (EKS, VPC, EC2, IAM, Subnets, NAT Gateway)
- **Modules:** terraform-aws-modules/eks, terraform-aws-modules/vpc

## Files
| File | Purpose |
|------|---------|
| `vpc.tf` | VPC, subnets, NAT gateway, routing |
| `eks.tf` | EKS cluster and node group config |
| `variables.tf` | Input variables |
| `providers.tf` | AWS provider configuration |
| `terraform.tf` | Terraform version constraints |

## How to use
```bash
# 1. Configure AWS credentials
aws configure

# 2. Initialise Terraform
terraform init

# 3. Review what will be created
terraform plan

# 4. Provision the infrastructure
terraform apply

# 5. Destroy when done
terraform destroy
```

## Prerequisites
- Terraform >= 1.0
- AWS CLI configured with sufficient IAM permissions
- kubectl installed for cluster access after provisioning

## What I learned
- How to structure modular Terraform for reusable infrastructure
- Why Spot Instances require careful configuration of 
  multiple instance types to avoid interruptions
- The difference between public and private subnet routing 
  for EKS worker nodes
