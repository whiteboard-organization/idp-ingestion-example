# Whiteboard Cluster - EKS Setup

This repository contains the Terraform configuration to deploy an Amazon EKS (Elastic Kubernetes Service) cluster named `whiteboard-cluster`. It provisions the cluster with a set of public and private subnets, managed node groups, and a set of essential Kubernetes add-ons.

## Requirements

- Terraform 0.12 or later
- AWS CLI configured with appropriate credentials
- AWS account with permissions to create and manage VPCs, subnets, EKS clusters, etc.

## Resources Created

- EKS Cluster: `whiteboard-cluster`
- Public Subnets: `subnet-0def8fc542de981c7`, `subnet-0dfb24dca12819039`, `subnet-0cc0ee88fb83773bf`
- Private Subnets: `subnet-01c2c3db799fb138c`, `subnet-078ef2d083eecda46`, `subnet-04d5cef83029d2c95`
- Managed Node Groups: 
  - `whiteboard-cluster-wg` with `t3.micro` instance types in Spot capacity.
  
## Local Variables

The following local variables are defined:

- `name`: The name of the EKS cluster (`whiteboard-cluster`).
- `region`: The AWS region where resources are deployed (`eu-north-1`).
- `public_subnets`: A list of public subnets for the cluster.
- `private_subnets`: A list of private subnets for the cluster.
- `tags`: Custom tags for resources, including an example tag.

## EKS Module Configuration

The configuration uses the `terraform-aws-modules/eks/aws` module to create the EKS cluster. It also sets up the following configurations:

- **Cluster Name**: `whiteboard-cluster`
- **Cluster Endpoint Public Access**: Enabled
- **Cluster Addons**: 
  - `coredns`, `kube-proxy`, and `vpc-cni` are configured to use the most recent versions.
- **VPC and Subnet Configuration**:
  - Uses the specified `vpc_id` and subnet IDs for private and public subnets.

## Node Group Configuration

The EKS managed node group `whiteboard-cluster-wg` is configured as follows:

- **Min Size**: 1
- **Max Size**: 2
- **Desired Size**: 2
- **Instance Type**: `t3.micro`
- **Capacity Type**: `SPOT`
- **Additional Tags**: `ExtraTag = whiteboard starter`

## Usage

### 1. Initialize Terraform

Run the following command to initialize Terraform and download the necessary providers and modules:

```bash
terraform init
terraform plan
terraform destroy
