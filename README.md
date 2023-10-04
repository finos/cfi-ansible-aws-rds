WORK IN PROGRESS


# CFI Service Deployment, Validation and Deletion for AWS RDS

The purpose of this repository is to provide an AWS RDS service that meets the security policies defined by ?? and docuemnted in the ??

-[] Ask on Validation working group where we are going to document the servce hardening

Testing of the IaC is achieved through a three step GitHub workflow, this workflow is trigged when code is pushed to the dev branch. The three steps are *deploy*, *validate* and *destroy*. 

This workflow uses [Ansible playbooks](ansible), these playbook are using the [AWS RDA Ansible Module](https://docs.ansible.com/ansible/latest/collections/amazon/aws/rds_instance_module.html). 
Playbooks could be reused by users as a starting to point to create a [CCC](https://www.finos.org/common-cloud-controls-project) and [CFI](https://github.com/finos/compliant-financial-infrastructure) compliant RDS in their own environments. 

Code is only meege to main on successful completion of CI in the dev branch. 

## Deploy

This step deploys an AWS RDS and applies the security policies that have been defined BY WHO and documented in THE. The [Ansible create-rds-db.yaml playbook](ansible/create-rds-db.yaml) used in this CI step bith creates the RDS and also applies the required security policies. 

## Validate

This step uses that CFI Validator to check that the defined security policy for Relational Databases has been correctly aplied to the deployed AWS RDS. If a PR fails validation an issue will be opened by the project maintaiers and assigned back to the contributor for resolution. 

-[] How are we going to share results? What happens if a validatation fails?

## Destroy

This step destroys the RDS once validation has been completed, this step runs the [Ansible delete-rds-db.yaml playbook](ansible/delete-rds-db.yaml)

-[] Need to check / update destroy step so it deletes RDS in event of validation failing. 

## CI Setup

For the CI to work it requires for GitHub Action secrets to be setup in the repo. In the main repo these are maintain by FINOS, when working on a fork of the repo then these need to be managed locally.

### GitHub Action Secrets

The following secrets need to be setup in this GutHub repo

* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY
* MASTER_USERNAME
* MASTER_USER_PASSWORD

The MASTER_USERNAME and MASTER_USER_PASSWORD are the master RDS credentials used when the DB is created. 

### AWS Setup

So that the RDS can be created a default VPC and default subnet need to be created. The following steps can be used to create these when working on a fork of the repo.  this is manged by FINOS in the main repo or locally i the AWS region (us-east-2) is coded into the Ansible playbook.

To create a default VPC and Subnet use the AWS cli.

Using the AWS CLI first run the configure command:

This will require an AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY and a default AWS Region. In the Ansible playbooks we are using *us-east-2*.

```shell
aws configure
```
The create the default VPC, this will also create the default subnets. Once created they can be checked in the AWS Console. 

```shell
aws ec2 create-default-vpc
```

## Database Setup

The database setup is coded into the [create]{ansible/create-rds-db.yaml} Ansible playbook. The database parameters used are:

 * allocated_storage: 10 
 * DB_ENGINE: "mariadb"
 * DB_ID: "CFI-validator-db"

-[] Needs to be updated once we have updated playbook to include security policies. 

## Usage

To contribute to this repo:

* Fork repository
* Update GitHub Secrets to reflect your local setup
* Update dev branch and test updates locally
* Open PR to parent repo to contribute back. The project maintainers are responsble for testing and merging PR's


To use this repo:

* Fork repository
* Update GitHub Secrets to reflect your local setup
* Update CI / Playbooks to support use case
