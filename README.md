# CFI Service Deployment, Hardening, Validation and Deletion Repository

The purpose of this repository is to provide infrastructure as code to deploy, harden, test and destroy services.

Required credentials and secrets are held in GITHUB Action secrets for this repo. 

This example is for Amazons Relational Database Service (RDS). 

## Setup

### GitHub Action Secrets

The following secrets need to be setup in this GutHub repo

* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY
* MASTER_USERNAME
* MASTER_USER_PASSWORD

### AWS Setup

This example creates an RDS in the default VPC using the default subnet, the AWS region (us-east-2) is coded into the Ansible playbook.

To create a default VPC and Subnet use the AWS cli.

```shell
aws configure
```
Then add WS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY and default AWS Region (us-east-2)

```shell
aws ec2 create-default-vpc
```

## Database Setup

The database setup is coded into the [create]{ansible/create-rds-db.yaml} Ansible playbook. The database parameters used are:

 * allocated_storage: 10 
 * DB_ENGINE: "mariadb"
 * DB_ID: "CFI-validator-db"


## Workflow

There is a GitHub Workflow that has been setup that include four jobs:

1. Deploy: Creates the hardened RDS
2. Validate: Runs the CFI Validator to ensure deploy services meets required policies
3. Destroy: Deletes the RDS

This workflow is trigger when a *push* is made to the Dev branch. The main and dev branches of this repo are write protected.


## Usage

TODO: Create this usage section


Link to [aws rds ansible module](https://docs.ansible.com/ansible/latest/collections/amazon/aws/rds_instance_module.html)
