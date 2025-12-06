# 🚀 Two-Tier Architecture Deployment with Ansible

# 📋 Table of Contents

* Project Overview
* Architecture Diagram
* Prerequisites
* Project Structure
* Configuration Files
* Deployment Steps
* Verification
* Troubleshooting

# 🎯 Project Overview

This project automates the deployment of a two-tier web application architecture using Ansible. It provisions and configures:

* Frontend Tier: Nginx web server with PHP-FPM support

* Backend Tier: MariaDB database server

* Infrastructure: AWS EC2 instances with secure SSH connectivity

# 🔧 Key Features

* ✅ Infrastructure as Code: Complete infrastructure definition in Ansible playbooks

* ✅ Automated Deployment: One-command deployment of both tiers

* ✅ Idempotent Operations: Safe to run multiple times

* ✅ Cross-Platform Compatibility: Works across different Linux distributions

* ✅ Customizable Configuration: Easy to modify for different requirements

* ✅ Health Monitoring: Built-in verification steps

# 📊 Architecture Diagram

```txt
2-tier-architecture-using-ansible/
│
├── inventory.ini                 # Inventory file with server details
├── 2-tier-architecture.yml       # Main Ansible playbook
├── Mumbai_server_key.pem         # SSH private key (in .gitignore)
├── README.md                     # This documentation file

```

# 🛠️ Prerequisites

## 💻 System Requirements

* Component Specification Notes
* Ansible Controller CentOS/RHEL 8+ or Ubuntu 20.04+ Control machine
* Web Server 1+ vCPU, 2GB RAM, 10GB storage Amazon Linux 2023
* Database Server 1+ vCPU, 2GB RAM, 20GB storage Amazon Linux 2023
* Network VPC with proper security groups Allow SSH (22), HTTP (80), MySQL (3306)


# 📁 Project Structure

```text
2-tier-architecture-using-ansible/
│
├── inventory.ini                 # Inventory file with server details
├── 2-tier-architecture.yml       # Main Ansible playbook
├── Mumbai_server_key.pem         # SSH private key (in .gitignore)
├── README.md                     # This documentation file
└── group_vars/                   # Group variables
    ├── webserver.yml
    └── dbserver.yml
```

# ⚙️ Configuration Files

## 🔐 Inventory File (inventory.ini)

```ini
# Web Server Configuration
[webserver]
43.204.238.50 ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/Mumbai_server_key.pem

# Database Server Configuration  
[dbserver]
13.235.75.175 ansible_user=ec2-user ansible_ssh_private_key_file=/home/ec2-user/Mumbai_server_key.pem
```

# 🚀 Deployment Steps

## Step 1: Clone and Prepare

```bash
# Clone repository (if applicable)
git clone <repository-url>
cd 2-tier-architecture-using-ansible

# Set proper permissions for SSH key
chmod 600 Mumbai_server_key.pem

# Create Ansible configuration
cat > ansible.cfg << EOF
[defaults]
host_key_checking = False
inventory = ./inventory.ini
remote_user = ec2-user
private_key_file = ./Mumbai_server_key.pem
EOF
```

## Step 2: Verify Connectivity

```bash
# Test SSH connectivity
ansible all -m ping -i inventory.ini

# Test with specific groups
ansible webserver -m ping -i inventory.ini
ansible dbserver -m ping -i inventory.ini

# Check system information
ansible all -m setup -a "filter=ansible_distribution*" -i inventory.ini
```

## Step 3: Validate Playbook Syntax

```bash
# Syntax check
ansible-playbook -i inventory.ini 2-tier-architecture.yml --syntax-check

# Dry run (simulation)
ansible-playbook -i inventory.ini 2-tier-architecture.yml --check --diff
```

## Step 4: Execute Deployment

```bash
# Full deployment
ansible-playbook -i inventory.ini 2-tier-architecture.yml

# Verbose output for debugging
ansible-playbook -i inventory.ini 2-tier-architecture.yml -vvv

# Run specific tags (if defined)
ansible-playbook -i inventory.ini 2-tier-architecture.yml --tags "webserver"
```

# ✅ Verification

## Web Server Verification

```bash
# Check web server accessibility
curl http://43.204.238.50
# Expected: HTML page with server hostname

# Check service status
ansible webserver -m shell -a "systemctl status nginx" -i inventory.ini
ansible webserver -m shell -a "systemctl status php-fpm" -i inventory.ini

# Verify installed packages
ansible webserver -m shell -a "nginx -v" -i inventory.ini
ansible webserver -m shell -a "php --version" -i inventory.ini
Database Server Verification
```bash
# Check database service
ansible dbserver -m shell -a "systemctl status mariadb" -i inventory.ini

# Verify database creation
ansible dbserver -m shell -a "mysql -u root -e 'SHOW DATABASES;'" -i inventory.ini
# Should show 'my_database' in the list

# Check MariaDB version
ansible dbserver -m shell -a "mysql --version" -i inventory.ini
```

# 🐛 Troubleshooting Guide

Debug Commands

```bash
# Increase verbosity
ansible-playbook -i inventory.ini playbook.yml -vvv

# Test specific task
ansible-playbook -i inventory.ini playbook.yml --tags "install_nginx"

# Check host connectivity
ansible -i inventory.ini all -m ping
```

# 📚 Resources & References

## 📖 Official Documentation

```txt
* Resource Link Description
* Ansible Documentation docs.ansible.com Official Ansible documentation and guides
* Ansible Module Index Ansible Module Index Complete list of Ansible modules
* Nginx Documentation nginx.org Nginx configuration and administration
* MariaDB Documentation mariadb.com/docs MariaDB server documentation
* PHP Documentation php.net/manual PHP programming language reference
* Amazon Linux 2023 AWS Documentation Amazon Linux 2023 user guide
```

# 📌 Author

Developed by Rushikesh Panchal – Cloud/DevOps Engineer 🚀
