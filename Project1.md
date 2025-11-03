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
   - **t2.micro** (Free tier eligible, suitable for testing)
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

<img width="1337" height="579" alt="EC2 with SG" src="https://github.com/user-attachments/assets/2448e303-47c1-4516-a723-34b13687d310" />

---

## 🔐 Step 2: Connect to Your Instance via SSH and Update the System

1. Open your terminal (or use **Git Bash** on Windows).  
2. Navigate to the directory where your **key pair (.pem)** file is stored.  
3. Run the following command to connect to your EC2 instance:

   ```bash
   ssh -i "your-key-name.pem" ubuntu@<Public-IP-Address>
   ```

4. Update your system packages:

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

<img width="951" height="364" alt="update " src="https://github.com/user-attachments/assets/b3d4214d-fe30-4a59-a2b6-606a1af4028a" />

---

## 🌐 Step 3: Install Apache Web Server

Apache is one of the most popular and reliable web servers used to host websites and web applications.  
We’ll install and verify that it’s running correctly on your EC2 instance.

### 🧩 Installation Steps

1. Make sure you’re still connected to your EC2 instance via SSH.

2. Install Apache using the following command:

   ```bash
   sudo apt install apache2 -y
   ```

3. Verify that Apache is running:

   ```bash
   sudo systemctl status apache2
   ```

✅ The green light indicates Apache2 is running.

<img width="1344" height="264" alt="install apache" src="https://github.com/user-attachments/assets/2ec31f60-10a8-4de3-bdad-d2b248a809d1" />

<img width="1342" height="308" alt="install apache2" src="https://github.com/user-attachments/assets/ccc1731e-b2ca-4e58-a773-0726ce90295b" />

---

## 🗄️ Step 4: Install MySQL

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

---

## 💡 Step 5: Install PHP

To install PHP and the required modules, run:

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

<img width="1341" height="376" alt="install php" src="https://github.com/user-attachments/assets/e6537e9d-44ad-4bcf-b322-cc158957e4ca" />

Verify the PHP version:

```bash
php -v
```

<img width="620" height="132" alt="php v" src="https://github.com/user-attachments/assets/5c25eae7-ab69-4ada-81e4-7c492b2a6af1" />

At this point, your LAMP stack is completely installed and fully operational.

---

## ⚙️ Step 6: Creating a Virtual Host for Your Website Using Apache

### Steps

1. Create the directory for your website:

   ```bash
   sudo mkdir /var/www/projectlamp
   ```

2. Assign ownership of the directory:

   ```bash
   sudo chown -R $USER:$USER /var/www/projectlamp
   ```

3. Create a new configuration file:

   ```bash
   sudo vi /etc/apache2/sites-available/projectlamp.conf
   ```

4. Add the following configuration:

   ```apache
   <VirtualHost *:80>
       ServerName projectlamp
       ServerAlias www.projectlamp
       ServerAdmin webmaster@localhost
       DocumentRoot /var/www/projectlamp
       ErrorLog ${APACHE_LOG_DIR}/error.log
       CustomLog ${APACHE_LOG_DIR}/access.log combined
   </VirtualHost>
   ```

<img width="466" height="176" alt="php with apache" src="https://github.com/user-attachments/assets/f238852b-35ff-4237-aaa9-17a8fc05887a" />

5. Save and close (`Esc` → `:wq` → `Enter`).  
6. Enable the site and disable the default one:

   ```bash
   sudo a2ensite projectlamp
   sudo a2dissite 000-default
   sudo apache2ctl configtest
   sudo systemctl reload apache2
   ```

7. Create a test HTML file:

   ```bash
   sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
   ```

8. Reload the page using your EC2 public IP.

<img width="1366" height="238" alt="the website" src="https://github.com/user-attachments/assets/62575d5b-4da4-4fa9-812d-10335dc4a191" />

---

## 🧾 Step 7: Enable PHP on the Website

1. Edit the `dir.conf` file:

   ```bash
   sudo vim /etc/apache2/mods-enabled/dir.conf
   ```

2. Update the order of file precedence:

   ```apache
   <IfModule mod_dir.c>
       DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
   </IfModule>
   ```

3. Save and close, then reload Apache:

   ```bash
   sudo systemctl reload apache2
   ```

4. Create a PHP test file:

   ```bash
   sudo vim /var/www/projectlamp/index.php
   ```

5. Add:

   ```php
   <?php
   phpinfo();
   ?>
   ```

6. Save and refresh your site.

<img width="1255" height="627" alt="the website by php" src="https://github.com/user-attachments/assets/617d9d2f-8f81-440a-9da6-500bc0fdec0b" />

---

## 🎯 Conclusion

You have successfully deployed a **LAMP Stack** on an **AWS EC2 instance**.  
Your environment is now ready to host and run **PHP-based web applications** with **MySQL** and **Apache** on **Ubuntu Linux**.
