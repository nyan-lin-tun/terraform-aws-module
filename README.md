# Terraform AWS Infrastructure Module

This Terraform module provisions a Virtual Private Cloud (VPC) and related AWS infrastructure components. It is designed to create a flexible and reusable setup for deploying AWS resources, including networking, bastion hosts, and SSH key pairs.

## Features

This module provides the following features:

### Networking
- **VPC**: Creates a Virtual Private Cloud (VPC) with customizable CIDR blocks.
- **Subnets**: Provisions public and private subnets across availability zones.
- **Internet Gateway**: Attaches an Internet Gateway to the VPC for public internet access.
- **NAT Gateways**: Creates NAT Gateways for private subnet internet access.
- **Route Tables**: Configures route tables and associations for public and private subnets.
- **S3 VPC Endpoint**: Adds an S3 Gateway Endpoint for private access to S3.

### Bastion Host
- **EC2 Instance**: Deploys a bastion host in the public subnet for secure SSH access to private resources.
- **Security Groups**: Configures security groups to allow SSH access to the bastion host and outbound traffic.

### SSH Key Pair
- **Key Pair**: Optionally creates an EC2 SSH key pair for secure access to instances.

## File Structure

- **`bastion.tf`**: Provisions the bastion host and its associated resources.
- **`data.tf`**: Fetches AWS region, caller identity, and availability zones.
- **`keypair.tf`**: Manages the creation of an EC2 SSH key pair.
- **`outputs.tf`**: Exports key outputs such as VPC ID, subnet IDs, and bastion host details.
- **`variables.tf`**: Defines input variables for customizing the module.
- **`versions.tf`**: Specifies the required Terraform and AWS provider versions.
- **`vpc.tf`**: Configures the VPC, subnets, NAT gateways, and route tables.
- **`templates/bastion_user_data.sh.tpl`**: User data script for initializing the bastion host.

## Inputs

The module accepts the following input variables (defined in `variables.tf`):

- **`friendly_name_prefix`**: Prefix for naming and tagging resources.
- **`common_tags`**: Map of common tags applied to all resources.
- **`create_vpc`**: Boolean to enable or disable VPC creation.
- **`vpc_name`**: Name of the VPC.
- **`vpc_cidr`**: CIDR block for the VPC.
- **`public_subnet_cidrs`**: List of CIDR blocks for public subnets.
- **`private_subnet_cidrs`**: List of CIDR blocks for private subnets.
- **`create_bastion`**: Boolean to enable or disable bastion host creation.
- **`bastion_ec2_keypair_name`**: Name of the SSH key pair for the bastion host.
- **`bastion_cidr_allow_ingress_ssh`**: List of CIDR ranges allowed to SSH into the bastion host.
- **`create_ec2_ssh_keypair`**: Boolean to enable or disable EC2 SSH key pair creation.
- **`ec2_ssh_keypair_name`**: Name of the EC2 SSH key pair.
- **`ec2_ssh_public_key`**: Public key material for the EC2 SSH key pair.

## Outputs

The module exports the following outputs (defined in `outputs.tf`):

- **`vpc_id`**: ID of the created VPC.
- **`private_subnet_ids`**: List of private subnet IDs.
- **`public_subnet_ids`**: List of public subnet IDs.
- **`bastion_public_dns`**: Public DNS name of the bastion host.
- **`bastion_public_ip`**: Public IP address of the bastion host.
- **`bastion_private_ip`**: Private IP address of the bastion host.

## Usage

```hcl
module "aws_infra" {
  source = "./path-to-this-module"

  friendly_name_prefix          = "my-project"
  create_vpc                    = true
  vpc_cidr                      = "10.0.0.0/16"
  public_subnet_cidrs           = ["10.0.1.0/24", "10.0.2.0/24"]
  private_subnet_cidrs          = ["10.0.3.0/24", "10.0.4.0/24"]
  create_bastion                = true
  bastion_ec2_keypair_name      = "my-keypair"
  bastion_cidr_allow_ingress_ssh = ["0.0.0.0/0"]
  create_ec2_ssh_keypair        = true
  ec2_ssh_public_key            = "ssh-rsa AAAAB3..."
}
```

## Requirements
- Terraform >= 1.8
- AWS Provider >= 5.54.0