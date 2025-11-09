# 📚 MEAN Stack Book Register App

## 🧩 Project Overview  
This project demonstrates how to build a **full-stack web application** using the **MEAN stack**, which consists of:  
- **MongoDB** – for storing book data in a NoSQL database  
- **Express.js** – to handle routes and server logic  
- **AngularJS** – to create the front-end and interact with the backend API  
- **Node.js** – to run the backend server  
    <img width="336" height="258" alt="image" src="https://github.com/user-attachments/assets/35a2f143-32e6-4032-818c-38d7cb16963b" />

The application allows users to **add**, **view**, and **delete books** from a MongoDB database through a simple web interface.  

---

## ⚙️ Project Steps

1. **Set Up the Environment**  
2. **Install Node.js and Express**  
3. **Set Up Server and Routing**  
4. **Build Front-End with AngularJS**  
5. **Run and Test the Application**  
---
## 🧩 Step 1 — Set Up the Environment

In this step, we prepare the server environment by updating the system, installing essential packages, and setting up the MongoDB repository.

---

### 1️⃣ Connect to the Server via SSH
```bash
ssh -i "your-key.pem" ubuntu@<PUBLIC_IP>
```
Replace "your-key.pem" and <PUBLIC_IP> with your actual values.

###2️⃣Update and Upgrade the System
```bash
sudo apt update
sudo apt upgrade -y
```
###3️⃣To import the MongoDB public GPG key, run the following command:
```bash
curl -fsSL https://pgp.mongodb.com/server-8.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg \
   --dearmor
```
###4️⃣Create the list file for Ubuntu 24.04 (Noble):
```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.com/apt/ubuntu noble/mongodb-enterprise/8.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise-8.2.list
```
###5️⃣Issue the following command to reload the local package database:
```bash
sudo apt-get update
```
###6️⃣To install the latest release of MongoDB Enterprise Server, run the following command:
```bash
sudo apt-get install mongodb-enterprise
```
  <img width="803" height="282" alt="4-install_db" src="https://github.com/user-attachments/assets/3d0f66d0-bb63-4c50-86d4-dcaa4451b2e8" />
  
**Start MongoDB:**
```bash
sudo systemctl daemon-reload
sudo systemctl start mongod
```
  
**Verify that MongoDB has started successfully:**
   ```bash
   sudo systemctl status mongod
   ```
<img width="835" height="184" alt="4-" src="https://github.com/user-attachments/assets/0a17c40a-d163-4ba2-b337-8257b583e64d" />

## ⚙️ Step 2 — Install Node.js and Initialize the Project

In this step, we install Node.js (the JavaScript runtime), initialize our project directory, and set up the required dependencies.

---

### 1️⃣ Install Node.js and npm
First, update the system and install Node.js using the NodeSource repository to ensure you get the latest LTS version:

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
```
  <img width="728" height="318" alt="1-install nodjes" src="https://github.com/user-attachments/assets/3f6733e0-b532-45f8-94f4-25d01f72e8ae" />

###2️⃣Install Node.js on the server with this command:
  ```bash
  sudo apt-get install -y nodejs
  ```
 *This command installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules &   packages and to manage dependency conflicts. 
   <img width="799" height="313" alt="2-" src="https://github.com/user-attachments/assets/8db93f9e-153c-4f74-9dd9-41daa2809b06" />

 * Verify the node installation with this command:
  ```bash
  node -v
  ```
* Verify the npm installation with this command:
  ```bash
  npm -v
  ```
    <img width="340" height="74" alt="3-" src="https://github.com/user-attachments/assets/d38880fa-8f91-4df3-ac51-82ab559684d4" />
    
  * Install npm – Node package manager: `sudo apt install -y npm`
  * Next we install body-parser package to help with processing JSON files passed in requests to the server. Use the following command: `sudo npm install body-parser`
  <img width="521" height="138" alt="5-" src="https://github.com/user-attachments/assets/5f7050bc-49ae-41bd-81a6-63f387c0d6b8" />

###3️⃣ Create a Project Directory:
*Create a folder for the project and navigate into it:
```bash
mkdir Books
cd Books
```
###4️⃣Initialize a new Node.js project:
```bash
npm init -y
```
*This creates a package.json file with default settings.
 <img width="581" height="651" alt="6-backend configrations" src="https://github.com/user-attachments/assets/e8c84cc0-8107-4949-a778-7d3643e2cf1e" />

###5️⃣Install Express and Mongoose

*Install the required packages:
```bash
npm install express mongoose
```
 <img width="711" height="134" alt="8-install_mangous" src="https://github.com/user-attachments/assets/3354a0db-3522-4366-b265-ce4dc1a0e3ab" />

 ## ⚙️ Step 3 — Create the Server File (server.js)

In this step, we’ll create the main backend server using **Express.js** to handle incoming requests and connect to **MongoDB**.

---

### 1️⃣ Create the Server File

*Create a new file named `server.js` inside your project directory:

```bash
touch server.js & nano server.js
```
*Paste the following code inside server.js:
```bash
 var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});
```
 <img width="579" height="233" alt="7-server js_file" src="https://github.com/user-attachments/assets/a39b951e-ba30-4d16-bcf0-bef776e47e03" />

 * while in **Books** folder, create a directory named **apps** and navigate into it with:
   ```bash
   mkdir apps && cd apps
   ```
 * Inside **apps**, create a file named routes.js with:
    ```bash
    vi routes.js
    ```
 * Copy and paste the code below into routes.js
```bash
var Book = require('./models/book');
var path = require('path');

module.exports = function(app) {
  // Get all books
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if (err) throw err;
      res.json(result);
    });
  });

  // Add new book
  app.post('/book', function(req, res) {
    var book = new Book({
      name: req.body.name,
      isbn: req.body.isbn,
      author: req.body.author,
      pages: req.body.pages
    });
    book.save(function(err, result) {
      if (err) throw err;
      res.json({
        message: "Successfully added book",
        book: result
      });
    });
  });

  // Delete book
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove({ isbn: req.params.isbn }, function(err, result) {
      if (err) throw err;
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    });
  });

  // Serve frontend (AngularJS)
  app.use((req, res) => {
    res.sendFile(path.join(__dirname, '../public', 'index.html'));
  });
};
```
 <img width="532" height="513" alt="9-routs_file" src="https://github.com/user-attachments/assets/18d051d6-9174-4e35-a7cf-f4ad869a14c8" />

 * Also create a folder named **models** in the **apps** folder, then navigate into it:
   ```bash
   mkdir models && cd models
   ``
 * Inside **models**, create a file named **book.js** with:
   ```bash
   vi book.js
   ```
 * Copy and paste the code below into ‘book.js’
```bash
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);
```
 <img width="461" height="223" alt="10-book_file" src="https://github.com/user-attachments/assets/77c6f029-28f8-4067-89a5-63e55ddecc28" />

## 💻 Step 4 — Frontend Setup with AngularJS

In this step, we’ll connect our **Express.js** backend with a simple **AngularJS** frontend to display and manage books in the database.

---

### 1️⃣ Create the Public Folder

Move back to the project root directory and create a folder named **public**:

```bash
cd ../..
mkdir public && cd public
```
* Add a file named **script.js**:
  ```bash
  vi script.js
  ```
* And copy and paste the following code:
  
 ```bash
 var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});
```
 <img width="416" height="544" alt="11-skerpt_file" src="https://github.com/user-attachments/assets/9074b574-15dc-4da1-b915-52185e9681e0" />
 
* Also in **public** folder, create a file named **index.html**:
  ```bash
  vi index.html
  ```
* And and paste the foloowing html code below into it:
  
```bash
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>
```
 <img width="656" height="638" alt="12-indix_file" src="https://github.com/user-attachments/assets/5ab44c2c-ff23-4ab1-a017-80956ecb0c8e" />

 ## 💻 Step 5- Run and Test the Application:
*change the directory back up to **Books** using: 
```bash
cd ..
```
* Now, start the server by running this command:
   ```bash
  node server.js
   ```
*If all goes well server should be up and running and we can connect to it on port 3300.
* This however was not the case in my test, I kept on getting an error. see error in the picture below:
  <img width="642" height="61" alt="image" src="https://github.com/user-attachments/assets/b11c9384-f023-423d-b5a5-e031723d6181" />
  
* Next  accesse the HTML page over the internet via port 3300 using the public IP:
  ```bash
  http://<public IP>:3300/
  ```
   <img width="662" height="302" alt="13-test-1" src="https://github.com/user-attachments/assets/63d9f826-f938-4f99-adc5-48b7cfc1c59d" />

  * Finally I enter some data into the database and it reflected.
    <img width="617" height="279" alt="14-test-2" src="https://github.com/user-attachments/assets/cbe67226-5cc8-4065-b059-81a1b5abf622" />


  






 











