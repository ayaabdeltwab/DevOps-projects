
# 🌐 WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS

## 📘 Project Description
In this project, we implemented a **LEMP Stack (Linux, Nginx, MySQL, PHP)** on an AWS EC2 instance.  
The main objective was to deploy a lightweight, high-performance web application using Nginx as the web server, MySQL for database management, and PHP for dynamic content.  

This project demonstrates how to configure and integrate all components of the LEMP stack on a Linux server, including database connection, PHP integration with Nginx, and verifying dynamic content retrieval from a MySQL database.  

The project covers:
- Launching and configuring an EC2 instance in AWS.  
- Installing and setting up **Nginx Web Server**.  
- Installing and securing **MySQL Database Server**.  
- Installing and configuring **PHP-FPM** to process dynamic content.  
- Creating and testing a sample PHP application to verify the connection between Nginx and PHP.  
- Connecting PHP with MySQL to retrieve and display data from a sample database.  

---

## ⚙️ Prerequisites  
Before starting, ensure you have the following:
- An **AWS account** with permission to launch EC2 instances.  
- A **key pair (PEM file)** for SSH access.  
- Basic knowledge of **Linux command-line** operations.  
- A terminal or SSH client (e.g., Git Bash, PuTTY, VS Code Remote SSH).  
- Properly configured **Security Group Rules** allowing inbound traffic on:
  - Port 22 (SSH)  
  - Port 80 (HTTP)  
- Optional: Visual Studio Code for editing and managing files.  

---

## 🚀 Steps Overview  

### **Step 1 – Launch an EC2 Instance on AWS**  
- Choose **Ubuntu Server** as the OS.  
- Select an instance type (e.g., `t2.micro` – free tier).  
- Configure security groups to allow SSH (22) and HTTP (80).  
- Connect to the instance using your private key:  
  ```bash
  ssh -i "key.pem" ubuntu@<Public-IP>
  ```
<img width="1339" height="533" alt="image" src="https://github.com/user-attachments/assets/09048ecf-45e7-46a2-af21-95489f8d1461" />

---

### **Step 2 – Install and Configure Nginx**  
- Update package lists:  
  ```bash
  sudo apt update
  ```  
- Install Nginx:  
  ```bash
  sudo apt install nginx -y
  ```  
- Verify installation by visiting your instance’s public IP in a browser — the Nginx welcome page should appear.
  <img width="849" height="453" alt="install nginx" src="https://github.com/user-attachments/assets/eead7bb0-7c2c-40e2-8677-3c7d2e66516f" />


* The server is running and we can access it locally and from the Internet but to access it from the internet port 80 must be open to allow traffic from the internet in.
* To test that the server can be accessed locally from the instance run the curl command: **curl http://localhost:80** The output shows that the server is accessible from the local host.
<img width="582" height="344" alt="test nginx locally" src="https://github.com/user-attachments/assets/bc789eef-0664-4bf3-8719-6e234d843fc4" />

---

### **Step 3 – Install MySQL Server**  
- Install MySQL:  
  ```bash
  sudo apt install mysql-server -y
  ```  
- Secure MySQL installation:  
  ```bash
  sudo mysql_secure_installation
  ```
  <img width="798" height="306" alt="install my_sql" src="https://github.com/user-attachments/assets/0b47034c-3426-43fb-8353-2abbf2e56341" />

- Log in to the MySQL console:  
  ```bash
  sudo mysql
  ```
* Run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. run the follwing command:
  ```bash
  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
  ```
* Exit SQL shell by typing **exit** 
* Start interactive scripting to configure the validate password pluggin. Run:
 ```bash
sudo mysql_secure_installation
 ```
  and answer Y to all the prompts.
*  At the point you can change the password of your root user and also decide the level of password validation.
* When done, test to know if login to the console is possible. type:
    ```bash
  sudo mysql -p
  ```
<img width="559" height="218" alt="test the query" src="https://github.com/user-attachments/assets/825e6cd2-02fd-441c-a12b-ccfcab7a518e" />


* This would prompt you to enter root user password. Enter the choosen password and enter.
* type exit to exit MySQL console.


---

### **Step 4 – Install PHP and PHP-FPM**  
* PHP has to be installed to process code and generate dynamic content for the web server.
* Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. You’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies.
  
- Install PHP, PHP-FPM, and the MySQL PHP extension:  
  ```bash
  sudo apt install php-fpm php-mysql -y
  ```
  <img width="792" height="208" alt="install php" src="https://github.com/user-attachments/assets/22e03693-4937-40d2-9031-59f79426c5ce" />

- Configure Nginx to use PHP by editing your site config file:
* Now that we have PHP components installed. Next, I will configure Nginx to use them.
* Create the root web directory for your_domain as follows:
  ```bash
  sudo mkdir /var/www/projectLEMP
  ```
* Next, assign ownership of the directory with the $USER environment variable, which will reference your current system user:
  ```bash
  sudo chown -R $USER:$USER /var/www/projectLEMP
  ```
* Then, open a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:
  ```bash
  sudo nano /etc/nginx/sites-available/projectLEMP
  ```
  
* This will create a new blank file. Paste in the following bare-bones configuration:
```bash
server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
}
```
<img width="380" height="278" alt="sit_config" src="https://github.com/user-attachments/assets/beec2543-f6ec-45d0-82ec-42d779251695" />

* Activate the configuration by linking to the config file from Nginx’s sites-enabled directory, run:
  ```bash
  sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
  ```
- Ensure Nginx passes `.php` files to PHP-FPM.  
- Test and reload Nginx:  
  ```bash
  sudo nginx -t
  sudo systemctl reload nginx
  ```
<img width="759" height="94" alt="config php" src="https://github.com/user-attachments/assets/7c5717e3-317f-49ad-b354-9096587c1534" />

* We need to disable default Nginx host that is currently configured to listen on port 80, for this run:
  ```bash
  sudo unlink /etc/nginx/sites-enabled/default
  ```
* Next, reload Nginx to apply the changes:
  ```bash
  sudo systemctl reload nginx
  ```
<img width="508" height="56" alt="disable default Nginx" src="https://github.com/user-attachments/assets/e7d7bde9-d9ba-4a1a-9e30-15c4ad03def5" />

* The website is now active, but the web root /var/www/projectLEMP is still empty. Create an index.html file in that location so that we can test that the new server block works as expected:
```bash
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
<img width="850" height="57" alt="index html file" src="https://github.com/user-attachments/assets/130637b5-f7a4-4f44-a91c-d9d1ee28062a" />

```
* Open a browser and try to open the website URL using IP address:
  ```bash
  http://Public-IP-Address:80
  ```
---

### **Step 5 – Test PHP with Nginx**  
* Now that LAMP stack is completely installed and fully operational. We test it to validate that Nginx can correctly hand .php files off to your PHP processor.
* Open a new file called info.php within your document root in your text editor:
  ```bash
  sudo nano /var/www/projectLEMP/info.php
  ```  
- Add the following content:  
  ```php
  <?php
  phpinfo();
  ?>
  ```  
- Access in browser:  
  ```bash
  http://<Public-IP>/info.php
  ```  
- You should see the PHP info page confirming successful integration.  
<img width="948" height="599" alt="php_connect_test" src="https://github.com/user-attachments/assets/5f96e205-d5f8-42d4-a4fb-37358529ff66" />
Remove the created file, as it contains sensitive information about your PHP environment and your Ubuntu server:
```bash
sudo rm /var/www/your_domain/info.php
```
---

### **Step 6 – Retrieve Data from MySQL with PHP**  

* The goal in this step is to create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.
* Because the native MySQL PHP library mysqlnd currently doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.
* I will be creating a database named example_database and a user named example_user:
    
  ```sql
  CREATE DATABASE example_database;
  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord1@';
  GRANT ALL ON example_database.* TO 'example_user'@'%';
  FLUSH PRIVILEGES;
  ```  
- Create a simple table and insert data:  
  ```sql
  CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
  );
  ```
  <img width="734" height="591" alt="databas_config" src="https://github.com/user-attachments/assets/d144771e-6a45-4aaa-9705-0fb03f1fc028" />
  ```bash
  INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
  INSERT INTO example_database.todo_list (content) VALUES ("Finish LEMP setup");
  ```
<img width="688" height="580" alt="add databas items " src="https://github.com/user-attachments/assets/5db7d316-9b34-43e2-acc6-20c2154d0267" />

- Create the PHP file to display data:  
  ```bash
  sudo nano /var/www/projectLEMP/todo_list.php
  ```  
  Add the following code:  
  ```php
  <?php
  $user = "example_user";
  $password = "PassWord1@";
  $database = "example_database";
  $table = "todo_list";

  try {
    $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
    echo "<h2>TODO</h2><ol>";
    foreach($db->query("SELECT content FROM $table") as $row) {
      echo "<li>" . $row['content'] . "</li>";
    }
    echo "</ol>";
  } catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
  }
  ?>
  ```
- Access it in your browser:  
  ```
  http://<Public-IP>/todo_list.php
  ```  
  You should see a “TODO” list rendered dynamically from MySQL.
<img width="731" height="180" alt="the app is done" src="https://github.com/user-attachments/assets/89554d88-b484-4efe-8fd3-0209cdf735c1" />

---

## ✅ Verification  
- PHP info page confirms Nginx-PHP integration.  
- The TODO list page confirms successful database connectivity and query execution.  

---

## 🧠 Key Takeaways  
- LEMP stack provides better performance and scalability compared to LAMP.  
- Using **Nginx** as a reverse proxy improves request handling and efficiency.  
- **PHP-FPM** separates dynamic processing from web serving, enhancing resource management.  
- Database connectivity and dynamic content rendering are critical for modern web applications.  
