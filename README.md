# Ansible AWS Infrastructure Deployment

This repository contains Ansible playbooks to deploy a two-tier AWS infrastructure. The setup includes a **Virtual Private Cloud (VPC)**, **public and private subnets**, an **Application Load Balancer (ALB)**, **EC2 instances**, **Route 53 for DNS**, **AWS Certificate Manager for SSL/TLS security**, and various **security groups** to secure the environment.

---

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Infrastructure Overview](#infrastructure-overview)
3. [Configuration Files](#configuration-files)
4. [Deployment Steps](#deployment-steps)
5. [Outputs](#outputs)
6. [Security Considerations](#security-considerations)
7. [Cleanup](#cleanup)

---

## Prerequisites

Before deploying the infrastructure, ensure you have the following:

- **Ansible**: Install Ansible from [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
- **AWS CLI**: Install and configure the AWS CLI with the necessary credentials. Follow the instructions [here](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html).
- **AWS Account**: Ensure you have an AWS account with sufficient permissions to create the resources defined in the Ansible playbooks.
- **SSH Key Pair**: Create an SSH key pair in the AWS region where you plan to deploy the infrastructure. This key pair will be used for EC2 instances.
- **Valid Domain Name**: Required for setting up Route 53 and SSL certificates.

---

## Infrastructure Overview

The infrastructure is designed to be **highly available**, **scalable**, and **secure**. Below is a high-level overview of the components:

### **Networking**

- **VPC**: A Virtual Private Cloud with public and private subnets across two availability zones.
- **Public Subnet**: Hosts the bastion host for SSH access.
- **Private Subnets**: Host EC2 instances running the website.
- **Security Groups**: Restrict access to EC2 instances and ALB based on IP and protocols.

### **Compute & Load Balancing**

- **EC2 Instances**: Deployed in private subnets for security and managed by Ansible.
- **Application Load Balancer (ALB)**: Distributes incoming traffic across EC2 instances in the private subnets.
- **Route 53**: Provides DNS resolution for the website domain.
- **AWS Certificate Manager (ACM)**: Manages SSL/TLS certificates for secure access.

---

## Configuration Files

The repository includes the following Ansible configuration files:

### **Core Configuration**

- **`inventory.ini`**: Defines the target EC2 instances for Ansible.
- **`ansible.cfg`**: Configuration file for Ansible settings.
- **`deploy_jupiter.yml`**: Main playbook for deploying the Jupiter website.

### **Playbook Tasks**

- **`install_apache.yml`**: Installs Apache HTTP server.
- **`deploy_website.yml`**: Downloads and deploys the Jupiter website from GitHub.
- **`configure_firewall.yml`**: Configures security rules for EC2 instances.

---

## Deployment Steps

### 1. **Clone the Repository**

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. **Configure SSH Agent Forwarding**

Enable SSH agent forwarding to connect through the bastion host.

```bash
ssh-add ~/.ssh/<your-private-key>
ssh -A ec2-user@<bastion-host-ip>
```

### 3. **Run the Ansible Playbook**

Execute the playbook to deploy the website on EC2 instances.

```bash
ansible-playbook -i inventory.ini deploy_jupiter.yml
```

### 4. **Set Up Route 53 and SSL Certificate**

- Configure your domain in Route 53 to point to the ALB.
- Attach an SSL certificate using AWS Certificate Manager.

### 5. **Verify Deployment**

Access the website via the domain name configured in Route 53.

---


---

## Security Considerations

- **SSH Access**: SSH access to the EC2 instances is restricted via a bastion host with agent forwarding.
- **ALB Security**: The Application Load Balancer only allows HTTP/HTTPS traffic on ports 80 and 443.
- **Security Groups**: Ensure only necessary ports are open to reduce attack vectors.

---

## Cleanup

To avoid incurring unnecessary charges, terminate the infrastructure once it is no longer needed.

### 1. **Delete EC2 Instances**

### 2. **Remove Load Balancer & DNS Records**

- Delete the ALB and associated listeners.
- Remove Route 53 DNS records.

### 3. **Verify Deletion**

Ensure that all resources have been deleted by checking the AWS Management Console.

---

## Conclusion

This README provides a comprehensive guide to deploying and managing the AWS infrastructure using Ansible. The automated setup ensures high availability, security, and scalability for the Jupiter website.

