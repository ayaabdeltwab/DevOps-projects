# 🌐 WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

## 📘 Project Description
In this project, we implemented a **LAMP Stack (Linux, Apache, MySQL, PHP)** on an **AWS EC2 instance**.  
The goal is to deploy a simple web application on a virtual server using cloud infrastructure.  
This setup demonstrates how a full web stack can be configured from scratch, including server provisioning, software installation, and connectivity testing.

The project covers:
- Launching an EC2 instance in AWS.
- Installing and configuring Apache Web Server.
- Installing MySQL for database management.
- Setting up PHP to serve dynamic web pages.
- Hosting a sample PHP application to verify the stack functionality.

---

## ⚙️ Prerequisites
Before starting, ensure you have the following:
- An **AWS account** with permissions to create EC2 instances.
- A **key pair (PEM file)** to access the instance via SSH.
- **Basic knowledge** of Linux command-line operations.
- A **terminal or SSH client** (e.g., Git Bash, PuTTY, or VS Code Remote SSH).
- Security group rules allowing inbound traffic on:
  - **Port 22** (SSH)
  - **Port 80** (HTTP)
- Optional: **Visual Studio Code** for editing files and running commands.

---
## 🚀 Steps Overview
1. Launch an EC2 Instance on AWS.  
2. Connect to your instance via SSH.  
3. Install Apache Web Server.  
4. Install MySQL.  
5. Install PHP.  
6. Test the LAMP Stack setup.
---
## 🖥️ Step 1: Launch an EC2 Instance on AWS

1. Log in to your **AWS Management Console** and navigate to **EC2** under the “Compute” section.  
2. Click **Launch Instance** to start creating a new virtual server.  
3. Enter a **Name** for your instance (e.g., `LAMP-Server`).
4. Under **Application and OS Images (Amazon Machine Image - AMI)**, select:
   - **Ubuntu Server 22.04 LTS (Free Tier Eligible)**
5. Choose an **Instance Type**:
   - **t2.micro** (Free tier eligible, suitable for testing).
6. Under **Key Pair (login)**:
   - Create a new key pair or select an existing one (you will use the `.pem` file to SSH into your instance).
7. Configure **Network Settings**:
   - Allow **SSH (Port 22)** and **HTTP (Port 80)** inbound traffic.
8. Leave storage settings as default (8 GB is fine for testing).  
9. Click **Launch Instance** to create your EC2 server.

Once launched:
- Go to **Instances → Running Instances**  
- Copy your **Public IP Address** — you’ll need it later to connect via SSH.

---

### ✅ Verification
You should see your EC2 instance in the **running** state under the EC2 dashboard, similar to the screenshot below:

Example:<img width="1337" height="579" alt="EC2 with SG" src="https://github.com/user-attachments/assets/2448e303-47c1-4516-a723-34b13687d310" />

![EC2 Instance Running](./images/ec2-running.png)

---

