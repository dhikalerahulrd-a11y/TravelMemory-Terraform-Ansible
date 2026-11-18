# TravelMemory — MERN Application Deployment Using Terraform and Ansible

## 1. Project Overview

This project demonstrates the deployment of a TravelMemory MERN stack application on AWS using Terraform for infrastructure provisioning and Ansible for server configuration and application deployment.

The objective is to automate the deployment process while implementing a secure, maintainable infrastructure architecture.

## 2. Technology Stack

| Component | Technology |
|---|---|
| Cloud Provider | AWS |
| Infrastructure as Code | Terraform |
| Configuration Management | Ansible |
| Frontend | React.js |
| Backend | Node.js and Express.js |
| Database | MongoDB |
| Web Server | Nginx |
| Operating System | Ubuntu Linux |
| Version Control | Git and GitHub |

## 3. AWS Architecture

The infrastructure consists of:

- A custom AWS VPC
- Public and private subnets
- An Internet Gateway
- A NAT Gateway
- Public route table and private route table
- Web EC2 instance in the public subnet
- MongoDB EC2 instance in the private subnet
- Security Groups for controlled network access
- IAM role for EC2 management

### Application Flow

```text
                  Internet
                     |
                 HTTP Port 80
                     |
              Internet Gateway
                     |
                 AWS VPC
                     |
              Public Subnet
                     |
                Web EC2
                     |
              Nginx Web Server
                     |
          +----------+----------+
          |                     |
      React Frontend       Express Backend
                                |
                           Port 27017
                                |
                         Private Subnet
                                |
                           MongoDB EC2
```

The database is hosted in a private subnet and is not directly accessible from the public internet.

## 4. Repository Structure

```text
TravelMemory-Terraform-Ansible/
|
|-- TravelMemory/
|   |-- backend/
|   |-- frontend/
|
|-- terraform/
|-- terraform-new-account/
|
|-- ansible/
|   |-- inventory/
|   |-- group_vars/
|   |-- templates/
|   |-- site.yml
|   |-- backend-deploy.yml
|   |-- frontend-deploy.yml
|   |-- mongo-security.yml
|   |-- mongo-users.yml
|   |-- nginx-install.yml
|
|-- docs/
|   |-- screenshots/
|
|-- .gitignore
|-- README.md
```

Note: Sensitive inventory and Vault files are intentionally excluded from version control.

## 5. Terraform Infrastructure Provisioning

Terraform is used to provision AWS networking, compute, security, and IAM resources.

### Prerequisites

- AWS account
- AWS CLI configured with appropriate permissions
- Terraform installed
- SSH key pair

### Deployment Commands

```bash
cd terraform-new-account

terraform init
terraform fmt -check
terraform validate
terraform plan
terraform apply
```

### Terraform Outputs

Terraform provides the following infrastructure information:

- Web EC2 public IP
- Web EC2 private IP
- Database EC2 private IP
- VPC ID
- Application URL

Terraform state files, plan files, and credential files must not be committed to GitHub.

## 6. Ansible Configuration Management

Ansible is used to configure the EC2 instances and deploy the application.

### Ansible Responsibilities

1. Install required system packages.
2. Install and configure MongoDB.
3. Configure MongoDB authentication.
4. Create database users.
5. Install Node.js and npm.
6. Deploy the Express backend.
7. Install and configure Nginx.
8. Build and deploy the React frontend.
9. Configure application services.

### Connectivity Verification

```bash
cd ansible
ansible all -m ping --ask-vault-pass
```

### Deployment Playbooks

```bash
ansible-playbook site.yml --ask-vault-pass
ansible-playbook mongo-security.yml --ask-vault-pass
ansible-playbook mongo-users.yml --ask-vault-pass
ansible-playbook node-install.yml --ask-vault-pass
ansible-playbook backend-deploy.yml --ask-vault-pass
ansible-playbook nginx-install.yml --ask-vault-pass
ansible-playbook frontend-deploy.yml --ask-vault-pass
```

Run the playbooks in the order appropriate for the configured infrastructure and application dependencies.

## 7. MongoDB Configuration

MongoDB is installed on a private EC2 instance.

Security measures include:

- MongoDB authentication enabled
- Separate administrator and application users
- Credentials managed through Ansible Vault
- Restricted access through AWS Security Groups
- Database communication over the private network

## 8. Backend Deployment

The Express backend is deployed to the web EC2 instance.

A systemd service named `travelmemory-backend` manages the backend process.

### Service Verification

```bash
sudo systemctl status travelmemory-backend
```

### Health Check

```bash
curl http://127.0.0.1:3001/health
```

Expected successful response:

```json
{
  "status": "ok",
  "db": "connected"
}
```

## 9. Frontend Deployment

The React frontend is built and served through Nginx.

Nginx also acts as a reverse proxy for backend API requests.

### Nginx Verification

```bash
sudo nginx -t
sudo systemctl status nginx
```

The application can be accessed using the public IP address of the web EC2 instance after successful deployment.

## 10. Security Measures

- Private subnet for MongoDB
- Restricted SSH access
- Database Security Group restrictions
- Ansible Vault for sensitive credentials
- No secrets stored in GitHub
- Nginx reverse proxy
- IAM role for AWS resource management

## 11. Deployment Verification

The following tests are used to validate the deployment:

| Test | Expected Result |
|---|---|
| Terraform validation | Successful |
| EC2 instance provisioning | Instances running |
| Ansible connectivity | Both hosts reachable |
| MongoDB service | Active |
| MongoDB authentication | Successful |
| Node.js installation | Installed |
| Backend service | Active |
| Backend health endpoint | HTTP 200 and database connected |
| Nginx service | Active |
| React homepage | Loads successfully |
| Add Travel Experience | Data saved successfully |
| MongoDB verification | Travel record available |

## 12. Deployment Screenshots

Screenshots of infrastructure provisioning, configuration management, application deployment, and functional testing are maintained in the `docs/screenshots/` directory.

Evidence includes:

- AWS VPC and EC2 infrastructure
- Terraform outputs
- Security Groups and IAM
- Ansible connectivity
- MongoDB installation and authentication
- Backend health verification
- Nginx and React deployment
- TravelMemory homepage
- Adding and viewing travel experiences
- GitHub repository

## 13. Cleanup

To avoid unnecessary AWS charges, Terraform-managed resources can be destroyed after completing the assignment and capturing all required evidence.

```bash
cd terraform-new-account
terraform destroy
```

Review the destruction plan carefully before confirming. Ensure the correct Terraform state and AWS account are selected.

## 14. Project Repository

GitHub Repository:

https://github.com/dhikalerahulrd-a11y/TravelMemory-Terraform-Ansible

## 15. Conclusion

This project demonstrates the use of Terraform and Ansible to provision AWS infrastructure and automate deployment of a MERN stack application.

It covers infrastructure provisioning, server configuration, application deployment, database security, service management, and deployment verification.

---

**Author:** Rahul Dhikale  
**Project:** TravelMemory MERN Deployment  
**Tools:** AWS, Terraform, Ansible, MongoDB, Express.js, React.js, Node.js, Nginx, Git, GitHub
