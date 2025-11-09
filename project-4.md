`# 📚 MEAN Stack Book Register App

## 🧩 Project Overview  
This project demonstrates how to build a **full-stack web application** using the **MEAN stack**, which consists of:  
- **MongoDB** – for storing book data in a NoSQL database  
- **Express.js** – to handle routes and server logic  
- **AngularJS** – to create the front-end and interact with the backend API  
- **Node.js** – to run the backend server  

The application allows users to **add**, **view**, and **delete books** from a MongoDB database through a simple web interface.  

---

## ⚙️ Project Steps

1. **Set Up the Environment**  
   - Update system packages and install necessary certificates.  
   - Add MongoDB repository and install MongoDB.  
   - Verify MongoDB service is running.

2. **Install Node.js and Express**  
   - Install Node.js and npm (Node Package Manager).  
   - Create the main project folder \`Books\`.  
   - Initialize a Node.js project using \`npm init\`.  
   - Install essential dependencies like **express**, **mongoose**, and **body-parser**.

3. **Set Up Server and Routing**  
   - Create \`server.js\` to initialize the Express server.  
   - Configure routing logic inside \`apps/routes.js\`.  
   - Connect MongoDB using Mongoose and define the schema in \`apps/models/book.js\`.

4. **Build Front-End with AngularJS**  
   - Create a \`public\` folder containing \`index.html\` and \`script.js\`.  
   - Use AngularJS to fetch, add, and delete book records via API calls.  

5. **Run and Test the Application**  
   - Start the backend using \`node server.js\`.  
   - Access the web interface through \`http://<your_public_ip>:3300\`.  
   - Perform CRUD operations (Create, Read, Delete) on book records.  

---
