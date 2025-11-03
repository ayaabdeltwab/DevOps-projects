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
---
## 🔐 Step 2: Connect to Your Instance via SSH and Update the System

1. Open your terminal (or use **Git Bash** on Windows).  
2. Navigate to the directory where your **key pair (.pem)** file is stored.  
3. Run the following command to connect to your EC2 instance:

   ```bash
   ssh -i "your-key-name.pem" ubuntu@<Public-IP-Address>
<img width="951" height="364" alt="update " src="https://github.com/user-attachments/assets/b3d4214d-fe30-4a59-a2b6-606a1af4028a" />
---
## 🌐 Step 3: Install Apache Web Server

Apache is one of the most popular and reliable web servers used to host websites and web applications.  
We’ll install and verify that it’s running correctly on your EC2 instance.

---

### 🧩 Installation Steps

1. Make sure you’re still connected to your EC2 instance via SSH.

2. Install Apache using the following command:

   ```bash
   sudo apt install apache2 -y
   
<img width="1344" height="264" alt="install apache" src="https://github.com/user-attachments/assets/2ec31f60-10a8-4de3-bdad-d2b248a809d1" />
1-To run apache2 package installation: sudo apt install apache2

2-Next, verify that Apache2 is running as a service in the OS. run: sudo systemctl status apache2

3-The green light indicates Apache2 is running

<img width="1342" height="308" alt="install apache2" src="https://github.com/user-attachments/assets/ccc1731e-b2ca-4e58-a773-0726ce90295b" />

## Step 4: Install MySQL

To install MySQL, run the following command:
```bash
sudo apt install mysql-server -y
```

When the installation completes, check that MySQL is running as a service:
```bash
sudo systemctl status mysql
```

After finishing, test that you can log in to MySQL:
```bash
sudo mysql
```

<img width="1348" height="371" alt="install mySQL" src="https://github.com/user-attachments/assets/499c22e5-3a7f-4a04-882d-68aae5357841" />

Create a new user and grant it privileges:
```sql
CREATE USER 'lamp_user'@'localhost' IDENTIFIED BY 'StrongPassword123!';
GRANT ALL PRIVILEGES ON lamp_db.* TO 'lamp_user'@'localhost';
FLUSH PRIVILEGES;
```

<img width="1340" height="357" alt="create user on mySQL" src="https://github.com/user-attachments/assets/721846e0-3771-4587-8e23-1dc5b5541e9a" />
## Step 5: Install PHP

To install PHP and the required modules, run the following command:
```bash
sudo apt install php libapache2-mod-php php-mysql -y
<img width="1341" height="376" alt="install php" src="https://github.com/user-attachments/assets/e6537e9d-44ad-4bcf-b322-cc158957e4ca" />

When the installation completes, verify the PHP version:
php -v
<img width="620" height="132" alt="php v" src="https://github.com/user-attachments/assets/5c25eae7-ab69-4ada-81e4-7c492b2a6af1" />
At this point, your LAMP stack is completely installed and fully operational.

### STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
#### Steps
1. Setting up a domain called projectlamp. Create the directory for projectlamp using ‘mkdir’. Run: **sudo mkdir /var/www/projectlamp**
2. assign ownership of the directory with your current system user, run: **sudo chown -R $USER:$USER /var/www/projectlamp**
3. Next, create and open a new configuration file in Apache’s sites-available directory. Tpye: **sudo vi /etc/apache2/sites-available/projectlamp.conf**
4. This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:
**<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>**
  <img width="466" height="176" alt="php with apache" src="https://github.com/user-attachments/assets/f238852b-35ff-4237-aaa9-17a8fc05887a" />

  5.To save and close the file. Hit the esc button on the keyboard, Type :, Type wq. w for write and q for quit and Hit ENTER to save the file.
  6. use the ls command to show the new file in the sites-available directory: **sudo ls /etc/apache2/sites-available**
7. Next, use a2ensite command to enable the new virtual host: **sudo a2ensite projectlamp**
  8. Disable the default website that comes installed with Apache. type: **sudo a2dissite 000-default**
  9. Esure your configuration file doesn’t contain syntax errors, run: **sudo apache2ctl configtest**
  10. Finally, reload Apache so these changes take effect: **sudo systemctl reload apache2**
  
  12. The website is active, but the web root /var/www/projectlamp is still empty. Create an index.html file in that location so that we can test that the virtual host works as expected:
**sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html**
12. Relaod the public IP to see changes to the apache2 default page.
<img width="1366" height="238" alt="the websit " src="https://github.com/user-attachments/assets/62575d5b-4da4-4fa9-812d-10335dc4a191" />

ENABLE PHP ON THE WEBSITE
#### Steps
1. With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. To make index.php file tak precedence need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive.
2. Run: **sudo vim /etc/apache2/mods-enabled/dir.conf** then:
**<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>**
4. Save and close file.
5. Next, reload Apache so the changes take effect, type: **sudo systemctl reload apache2**
6. Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server. Create a new file named index.php inside the custom web root folder, run: **vim /var/www/projectlamp/index.php**
7. This will open a blank file. Add the PHP code: 
**<?php
phpinfo();**
8. Save and close the file, then refresh the page to see changes.
<img width="1255" height="627" alt="the websit by php " src="https://github.com/user-attachments/assets/617d9d2f-8f81-440a-9da6-500bc0fdec0b" />
