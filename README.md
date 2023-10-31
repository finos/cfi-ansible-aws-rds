[![FINOS - Incubating](https://cdn.jsdelivr.net/gh/finos/contrib-toolbox@master/images/badge-incubating.svg)](https://finosfoundation.atlassian.net/wiki/display/FINOS/Incubating)
[![Join us on Slack](https://img.shields.io/badge/slack-@finos/cfi%20runtime%20validation-green.svg?logo=slack)](https://finos-lf.slack.com/messages/cfi-runtime-validation-wg)

# CFI Ansible Playbook for AWS RDS

The purpose of this repository is to provide an AWS RDS service that meets the security policies defined by the FINOS Compliant Financial Infrastructure (CFI) Policy Working Group or FINOS Common Cloud Controls.

Validation of compliance with the security policies is measured after the IaC has been deployed, using the [RDS Raid](https://github.com/krumIO/raid-rds) for [Privateer](https://github.com/privateerproj) supplied by the CFI Runtime Validation Working Group.

Runtime Validation of the IaC is achieved through a three-step GitHub workflow, outlined below.

## CI Overview

This deploy->validate->destroy workflow can be triggered manually, or will run when code is merged to the dev branch.

For the CI to work it requires for GitHub Action secrets to be setup in the repo. In the main repo these are supplied by FINOS technical support. When working on a fork of the repo, these need to be managed by the repo owner.

### Deploy

This step deploys an AWS RDS and applies the security policies that have been selected. The [Ansible create-rds-db.yaml playbook](ansible/create-rds-db.yaml) used in this CI step both creates the RDS and also applies the required security policies.

### Validate

This step uses the RDS Raid to ensure that the defined security policy for Relational Databases has been correctly applied to the deployed AWS RDS. Any failing checks should be promptly investigated and resolved.

### Destroy

This step destroys the RDS once validation has been completed, this step runs the [Ansible delete-rds-db.yaml playbook](ansible/delete-rds-db.yaml)

## GitHub Action Secrets

The following secrets need to be setup in this GutHub repo

* AWS_KEY_ID
* AWS_KEY_SECRET
* MASTER_USERNAME
* MASTER_USER_PASSWORD

The MASTER_USERNAME and MASTER_USER_PASSWORD are the master RDS credentials used when the DB is created. 

## AWS Setup

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

The database setup is coded into the [create]{ansible/create-rds-db.yaml} Ansible playbook. 


### Basic DB Setup

The database parameters used are:

 * allocated_storage: 10 
 * DB_ENGINE: "mariadb"
 * DB_ID: "CFI-validator-db"


### DB Hardening

#### Encryption

STORAGE_ENCRYPTED: "true"

Storage encryption is enabled by the storage-encrpted variable. If no key is specified then an AWS key will be used. More detail can be found at this [link](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) in the AWS documentation.
  



### Usage

To contribute to this repo:

* Fork repository
* Update GitHub Secrets to reflect your local setup
* Update dev branch and test updates locally
* Open PR to parent repo to contribute back. The project maintainers are responsble for testing and merging PR's


To use this repo:

* Fork repository
* Update GitHub Secrets to reflect your local setup
* Update CI / Playbooks to support use case

## Join the Community!

You may request changes via [GitHub Issues](https://github.com/finos/compliant-financial-infrastructure/issues) (we recommend also raising discussion on Slack or in a community meeting to ensure your issue is properly prioritized).

Be sure to join our public community meetings found on the [FINOS community calendar](https://www.finos.org/finos-community-calendar), and join us on [Slack](https://finos-lf.slack.com/messages/cfi-reproducible-infrastructure-wg).

## Contributing

For more information about how to engage with the rest of the community and contribute to the project, view the documentation and links [here](CONTRIBUTING.md).

## License

Distributed under the [Apache License, Version 2.0](LICENSE).

SPDX-License-Identifier: [Apache-2.0](https://spdx.org/licenses/Apache-2.0)

## Security Concerns

If you have any security concerns related to this project, please [create an issue on the primary CFI repository](https://github.com/finos/compliant-financial-infrastructure/issues/new/choose) or privately report via [help@finos.org](mailto:help@finos.org).
