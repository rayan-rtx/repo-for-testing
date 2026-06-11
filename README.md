# Deploy Laravel App to EC2

![GitHub Actions](https://img.shields.io/badge/github%20actions-%232671E5.svg?style=for-the-badge&logo=githubactions&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![Terraform](https://img.shields.io/badge/terraform-%235835CC.svg?style=for-the-badge&logo=terraform&logoColor=white)
![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)

lorem lorem lorem lorem lorem lorem lorem lorem lorem lorem.

## Overview

The `deploy-laravel-app-to-ec2` repository is ... implements a the complete `CI/CD` pipeline for deploying a Laravel application using :

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

    ├── 📁 .github  
    ├── 📁 ansible
    ├── 📁 apache  
    ├── 📁 app  
    ├── 📁 docs  
    ├── 📁 scripts  
    ├── 📁 terraform  
    ├── 📄 docker-compose.yml  
    ├── 📄 Dockerfile  
    └── 📄 README.md  

## Requirements

**⩩ AWS account ( tab the [link](https://signin.aws.amazon.com/signup?request_type=register) and follow the steps ) :**<br><br>

![Alt Text](docs/images/aws/create-aws-account.png)<br><br>

**⩩ Key Pair ( from AWS panel ) :**<br><br>

![Alt Text](docs/images/aws/key-pair.png)<br><br>

**⩩ S3 bucket for terraform backend ( from AWS panel ) :**<br><br>

![Alt Text](docs/images/aws/s3-bucket.png)<br><br>

**⩩ DynamoDB table for terraform locking ( from AWS panel ) :**<br><br>

![Alt Text](docs/images/aws/dynamodb-table.png)<br><br>

**⩩ GitHub secrets keys ( tab the [link](https://github.com/<your-username>/<your-repo>/settings/secrets/actions) and follow the steps ) :**<br><br>

![Alt Text](docs/images/github-actions/repository-secret-variables.png)<br><br>

This is what these keys represent :

- `EC2_SSH_KEY` : Private SSH key used by Ansible to connect to the EC2 instance.
- `AWS_ACCESS_KEY` : AWS Access Key ID used to authenticate Terraform.
- `AWS_SECRET_KEY` : AWS Secret Access Key paired with the Access Key ID.
- `DB_USERNAME` : Master username for the RDS MySQL database. Terraform uses it when creating the RDS instance.
- `DB_PASSWORD` : Master password for the RDS MySQL database. Terraform uses it when creating the RDS instance.

## Deployment flow explained

**1. Push changes or use manual trigger :**

![Alt Text](docs/images/github-actions/manual-trigger-workflow.png)<br><br>

**2. Run GitHub Actions jobs sequentially :**

- This all steps executed after run `Build` job :

![Alt Text](docs/images/github-actions/jobs/build-job.png)<br><br>

- This all steps executed after run `Test` job :

![Alt Text](docs/images/github-actions/jobs/test-job.png)<br><br>

- This all steps executed after run `Deploy` job :

![Alt Text](docs/images/github-actions/jobs/deploy-job.png)<br><br>

**3. Terraform provisioning :**

- This is the EC2 instance we obtain after running the IAC ( Infrastructure as Code ) :

![Alt Text](docs/images/aws/ec2-terraform-provisioning-1.png)<br><br>


![Alt Text](docs/images/aws/ec2-terraform-provisioning-2.png)<br><br>

- This is the RDS database we obtain after running the IAC ( Infrastructure as Code ) :

![Alt Text](docs/images/aws/rds-terraform-provisioning-1.png)<br><br>


![Alt Text](docs/images/aws/rds-terraform-provisioning-2.png)<br><br>

**4. EC2 Access Configuration :**

- Create the SSH configuration directory.
- Retrieve the EC2 private key from GitHub Secrets.
- Add the EC2 host fingerprint to known hosts.
- Generate a dynamic Ansible inventory file.

**5. Ansible management :**

- Install Ansible on the GitHub Actions runner.
- Passe database configuration variables to Ansible.
- Execute the deployment playbook on the target EC2 instance.

**6. Dockerized application :**

- Install Docker Engine.
- Add the `ubuntu` user to the Docker group.
- Enable and start the Docker service.
- Build Docker images using Docker Compose.
- Start application containers in detached mode.
- Passe database connection variables to containers.

**7. Application access :**

- You can now access the EC2 instance via the web using public ip address ( in our case `13.60.43.122` ) :

![Alt Text](docs/images/web-page.png)<br><br>

- On the other hand, you can now access the PHPMyAdmin panel via the web using public ip address with port ( in our case `13.60.43.122:8080` ), and login using the credentials we previously knew ( `DB_USERNAME` and `DB_PASSWORD` ) :

![Alt Text](docs/images/pma-1.png)<br><br><br>
![Alt Text](docs/images/pma-2.png)<br><br><br>
![Alt Text](docs/images/pma-3.png)<br><br>

**⚠️ Important Note:**

It is not recommended to provide any method for access the PHPMyAdmin panel in the production environment ( we did this for illustrative purposes only ).

## Contributing

We welcome contributions! Please follow these guidelines :

- Fork the repository and create a new branch for your feature or fix.
- Write clear commit messages and document your code.
- Ensure all tests pass before submitting a pull request.
- Follow the established code style and project structure.
- Open an issue for discussion before major changes.

## License

This project is open-sourced under the [MIT License](LICENSE).

---

Thank you for using `deploy-laravel-app-to-ec2`! For questions or support, please open an issue on GitHub.
