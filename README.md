# Terraform Jenkins Project

This project demonstrates Infrastructure as Code (IaC) using **Terraform** to provision AWS resources, automate Docker container deployments, configure remote state management, and integrate with CI/CD pipelines.

## Project Scope

The following tasks are implemented in this project:

1. **Provision AWS Core Resources**
   - VPCs
   - Subnets
   - Security Groups
   - EC2 Instances

2. **Docker Container Deployment**
   - Automate Docker container deployment on EC2 instances.

3. **Modular Terraform Setup**
   - Create reusable modules for:
     - VPC
     - EC2 Instances
     - Security Groups
     - EKS (Elastic Kubernetes Service)

4. **Module Inputs and Outputs**
   - Pass variables between modules and utilize outputs effectively.

5. **Provision AWS EKS (Elastic Kubernetes Service)**
   - Provision a fully functional Kubernetes cluster using EKS.

6. **Configure EKS Node Groups and IAM Roles**
   - Set up managed node groups and necessary IAM roles and policies.

7. **Remote State Management**
   - Use AWS S3 bucket as remote storage for Terraform state files.
   - Configure DynamoDB for state locking to prevent concurrent changes.

8. **State Locking for Team Deployments**
   - Ensure infrastructure integrity by implementing state locking with DynamoDB.

9. **CI/CD Automation**
   - Automate server provisioning using Terraform as part of the CI/CD process.

10. **EC2 + Docker Integration**
    - Use Terraform to create EC2 instances and deploy Docker containers seamlessly.

11. **Jenkins Integration**
    - Integrate Terraform provisioning steps into a Jenkins pipeline (Jenkinsfile).

---

## Project Structure

```bash
.
├── modules/
│   ├── vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── ec2/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── security-groups/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── eks/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
├── main.tf                # Root module, calling all sub-modules
├── variables.tf           # Global variables (optional)
├── outputs.tf             # Global outputs
├── terraform.tfstate      # Terraform state (should be remote)
├── backend.tf             # Remote state backend config (S3 + DynamoDB)
├── jenkins/
│   └── Jenkinsfile        # Jenkins pipeline script
└── README.md              # Project documentation
```
## Prerequisites
- Terraform v1.x

- AWS CLI configured with appropriate credentials

- AWS account with permissions to create:

- VPCs, EC2, EKS, S3, DynamoDB, IAM, etc.

- Jenkins installed (for CI/CD steps)

- Docker installed on EC2 instances (optional: baked AMI or cloud-init)

## Usage
 ### 1. Initialize Terraform
``` bash
terraform init
```
### 2. Plan the Deployment
```bash
terraform plan
```
### 3. Apply the Configuration
```bash
terraform apply
```
### 4. Destroy the Infrastructure (Cleanup)
```bash
terraform destroy
```
## Remote State Configuration
The project uses an S3 bucket and DynamoDB table for remote state management and locking.

Example backend configuration:

```bash
terraform {
  backend "s3" {
    bucket         = "your-tf-state-bucket"
    key            = "path/to/terraform.tfstate"
    region         = "your-aws-region"
    dynamodb_table = "your-tf-lock-table"
  }
}
```
Ensure both the S3 bucket and DynamoDB table exist before running terraform init.

## CI/CD Pipeline
The Jenkins pipeline (Jenkinsfile) automates:

- Terraform formatting and validation

- Plan and Apply stages for infrastructure provisioning

- Docker deployment on provisioned EC2 instances

Ensure Jenkins has:

- AWS credentials configured

- Terraform and Docker installed

- Pipeline plugins enabled

## Notes
- Avoid pushing .terraform directories or state files to GitHub.

- Use .gitignore to exclude unnecessary files:

```bash
.terraform/
terraform.tfstate
terraform.tfstate.backup
```
- Use versioning and encryption for your S3 backend.



## Author
- Michael Oppong
- Email: michaeloppong731@gmail.com
- GitHub: MichaelOppong731



