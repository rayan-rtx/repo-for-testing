# Deploy Laravel App to EC2

![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

lorem lorem lorem lorem lorem lorem lorem lorem lorem lorem.

## Overview

The `deploy-laravel-app-to-ec2` repository is ... implements a the complete "CI/CD" pipeline for deploying a Laravel application using :

- Apache.
- Docker ( Docker Compose ).
- Terraform.
- Ansible.
- GitHub Actions.
- AWS ( EC2 + RDS + S3 + DynamoDB ).

## What this project demonstrates

- Automated CI/CD pipelines.
- Dockerized Laravel app.
- Infrastructure as code.
- Configuration Management.

## Project Structure

## Requirements

- AWS account (https://signin.aws.amazon.com/signup?request_type=register)
![Alt Text](docs/images/aws/create-aws-account.png)
- Key Pair
![Alt Text](docs/images/aws/key-pair.png)
- S3 bucket for terraform backend
![Alt Text](docs/images/aws/s3-bucket.png)
- DynamoDB table for terraform locking
![Alt Text](docs/images/aws/dynamodb-table.png)
- GitHub secrets keys (https://github.com/<your-username>/<your-repo>/settings/secrets/actions) :
![Alt Text](docs/images/github-actions/repository-secret-variables.png)
- EC2_SSH_KEY : Private SSH key used by Ansible to connect to the EC2 instance.
- AWS_ACCESS_KEY : AWS Access Key ID used to authenticate Terraform.
- AWS_SECRET_KEY : AWS Secret Access Key paired with the Access Key ID.
- DB_USERNAME : Master username for the RDS MySQL database. Terraform uses it when creating the RDS instance.
- DB_PASSWORD : Master password for the RDS MySQL database. Terraform uses it when creating the RDS instance.

## Deployment flow explained

**1.** Push changes to "main" branch or use manual trigger :
![Alt Text](docs/images/github-actions/manual-trigger-workflow.png)

**2.** Run GitHub Actions jobs sequentially :
- Build app
![Alt Text](docs/images/github-actions/jobs/build-job.png)
- Run tests
![Alt Text](docs/images/github-actions/jobs/test-job.png)
- Deploy app
![Alt Text](docs/images/github-actions/jobs/deploy-job.png)

3. Terraform provisioning :
- Create EC2 instance
![Alt Text](docs/images/aws/ec2-terraform-provisioning-1.png)
![Alt Text](docs/images/aws/ec2-terraform-provisioning-2.png)
- Create RDS
![Alt Text](docs/images/aws/rds-terraform-provisioning-1.png)
![Alt Text](docs/images/aws/rds-terraform-provisioning-2.png)

4. EC2 Access Configuration :
- Create the SSH configuration directory.
- Retrieve the EC2 private key from GitHub Secrets.
- Add the EC2 host fingerprint to known hosts.
- Generate a dynamic Ansible inventory file.

5. Ansible management :
- Install Ansible on the GitHub Actions runner.
- Passe database configuration variables to Ansible.
- Execute the deployment playbook on the target EC2 instance.

6. Dockerized application :
- Install Docker Engine.
- Add the `ubuntu` user to the Docker group.
- Enable and start the Docker service.
- Build Docker images using Docker Compose.
- Start application containers in detached mode.
- Passe database connection variables to containers.

7. Application access :
- EC2 web page
![Alt Text](docs/images/web-page.png)
- PHPMyAdmin web interface + security alert
![Alt Text](docs/images/pma-1.png)
![Alt Text](docs/images/pma-2.png)
![Alt Text](docs/images/pma-3.png)
