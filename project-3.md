# 📝 SIMPLE TO-DO APPLICATION USING MERN STACK

## 🌐 Overview
This project demonstrates the deployment of a **To-Do Application** built on the **MERN stack (MongoDB, Express, React, Node.js)**.  
The goal is to create a simple yet functional To-Do list app that allows users to:
- Add new tasks  
- View all existing tasks  
- Delete completed tasks  

The backend (Node.js + Express + MongoDB) handles API requests and data persistence, while the frontend (React) provides an interactive user interface.  
The entire project is deployed and tested on an **Ubuntu EC2 instance**.
<img width="693" height="384" alt="image" src="https://github.com/user-attachments/assets/b645a28a-9d2c-453d-b3ee-19cf8893e104" />

The application allows users to **add, update, and delete tasks** from a todo list, with persistent data stored in MongoDB.
---
## ⚙️ Step 1 — Setup the Backend (Node.js + Express + MongoDB):
* Update ubuntu:
```bash
 sudo apt update
```
* Upgrade ubuntu:
  ```bash
  sudo apt upgrade
  ```
  * Get the location of Node.js software from Ubuntu repositories. Run:
  ```bash
  curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
  ```
 <img width="717" height="265" alt="1-install nodjs" src="https://github.com/user-attachments/assets/8e5989bc-aaaf-4ba8-bc85-635b046570d3" />
 * Install Node.js on the server with this command:
  ```bash
  sudo apt-get install -y nodejs
  ```
 *This command installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules &   packages and to manage dependency conflicts. 
 
 * Verify the node installation with this command:
  ```bash
  node -v
  ```
* Verify the npm installation with this command:
  ```bash
  npm -v
  ```
  <img width="302" height="107" alt="2- -v" src="https://github.com/user-attachments/assets/b8a270b7-ee36-4639-b332-cac582726f3d" />

 ### 📦 Application Code Setup

1. **Create a new directory** for your To-Do project:  
   ```bash
   mkdir Todo
   ```
   *This command creates a new folder named Todo where all project files will be stored.
   Run the command below to verify that the Todo directory is created:
   ```bash
   ls
   ```
   *You should see the Todo folder listed in the output.
   Next, change the current directory to the newly created one:
   ```bash
   cd Todo
   ```
   *Initialize your Node.js project using npm init.
  This command creates a new file named package.json that holds important information about your project, such as name, version,    dependencies, and scripts.
  ```bash
  npm init
  ```
  *Follow the prompts in the terminal — you can press Enter to accept the defaults, and then confirm with yes when asked to write the file.
  <img width="560" height="478" alt="3-npm init " src="https://github.com/user-attachments/assets/0cf83d18-a167-4b00-9e27-976a93e90685" />
  *Run the command below to confirm that the file was created successfully:
  ```bash
  ls
  ```
##### INSTALL EXPRESSJS

* To use **Express**, install it using npm:  
  ```bash
  npm install express
  ```
  Next, create a file index.js with this command:
  ```bash
  touch index.js
  ```
  <img width="470" height="185" alt="5-config" src="https://github.com/user-attachments/assets/792eeebe-1a31-4601-83f9-7e98bcf2d7a7" />

  Next step is to install the dotenv module:
  ```bash
  vim index.js
  ```
  Type the code below into it and save:
```bash
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "*");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});

app.use((req, res, next) => {
  res.send('Welcome to Express');
});

app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
```
<img width="759" height="319" alt="4-index js-config" src="https://github.com/user-attachments/assets/288b8216-ed7a-4093-99f6-ef66024f85b5" />
*Use :w to save in vim and use :qa to exit vim.
* Test start the server to see if it works. Type:
  ```bash
  node index.js
  ```
* You should see Server running on port 5000 in the terminal.
  
  <img width="843" height="100" alt="6-" src="https://github.com/user-attachments/assets/2eb60696-dd33-4fc2-a389-a24354018781" />

* Next, open port 5000 in EC2 Security Groups and save changes.
* Open a browser and access the server’s Public IP or Public DNS name followed by port 5000:
  ```bash
  http://<PublicIP-or-PublicDNS>:5000
  ```
  
  <img width="418" height="138" alt="7-welcom" src="https://github.com/user-attachments/assets/b9bfe562-c77f-4b25-a8a4-b9e44e508eed" />

##### Routes
* The To-Do application needs to be able to complete 3 actions:
  * Create a new task
  * Display list of all tasks
  * Delete a completed task
* For each task, we need to create routes that will define various endpoints that the To-do app will depend on. Create a folder called **routes** with this command:
  ```bash
  mkdir routes
  ```
* Change directory to **routes** folder and create a file **api.js** with the command:
  ```bash
  touch api.js
  ```
* Open the file with the command below:
   ```bash
   vim api.js
   ```
* Copy and save the code below into the file:
```bash
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
```

 <img width="502" height="286" alt="8-rourts" src="https://github.com/user-attachments/assets/130cfa99-f5ee-46ff-825a-609e83e58187" />

 ##### MODELS
* To create a Schema and a model, install mongoose which is a Node.js package that makes working with mongodb easier. Change directory back Todo folder with:
   ```bash
   cd ..
   ```
  and install Mongoose with the following command:
  ```bash
  npm install mongoose
  ```
  <img width="436" height="120" alt="9-install-mongoose" src="https://github.com/user-attachments/assets/e88dfa0a-eea4-4ada-8d9a-2d5e5a364d8e" />

* Create a new folder **models**, then change directory into the newly created **models** folder, Inside the models folder, create a file and name it **todo.js** with the following command:
   ```bash
   mkdir models && cd models && touch todo.js
   ```
* Open the file created with vim todo.js then paste the code below in the file:
```bash
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
```
 <img width="672" height="267" alt="10-moudles" src="https://github.com/user-attachments/assets/b5780561-dd70-44fa-b1a4-8791c6eb0852" />
 
* Next, we update our routes from the file api.js in ‘routes’ directory to make use of the new model. In routes directory, open api.j with
  ```bash
  vim api.js
  ```
 delete the code inside with:
  ```bash
  :%d
  ```
* command and paste there code below into it then save and exit
  
```bash
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;
```
 <img width="672" height="547" alt="11-api-update" src="https://github.com/user-attachments/assets/06d9a470-6999-4f12-aed4-ad32473e38d1" />
 
##### MONGODB DATABASE
* A database is required where data will be stored. For this we will make use of mLab. Sign up for a shared clusters free account, Sign up on
   ```bash
   https://www.mongodb.com/atlas-signup-from-mlab.
   ```
* Follow the sign up process, select AWS as the cloud provider, and choose a region.
* For the purposes of this project, allow access to the MongoDB database from anywhere.
  <img width="1344" height="485" alt="14-database user" src="https://github.com/user-attachments/assets/b60e2e17-492f-4646-b580-8ff07dd4ac67" />

* Make sure you change the time of deleting the entry from 6 Hours to 1 Week
  <img width="1362" height="381" alt="15-database -ip" src="https://github.com/user-attachments/assets/1872a4f4-3da8-4048-af06-72bca28d2b41" />

* Create a MongoDB database and collection inside mLab
  <img width="1340" height="573" alt="12-database" src="https://github.com/user-attachments/assets/844f758e-53a7-4469-8d22-d9d7d45abf32" />

* Next, in the index.js file, we specified **process.env** to access environment variables, but we are yet to create the file. Now, create a file in the **Todo** directory and name it **.env** To do this type: 
```bash
touch .env
vi .env
```
* Then add the connection string to access the database in it, just as below:
  ```bash
  DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
  ```
* Update the **index.js** to reflect the use of **.env** so that Node.js can connect to the database. Delete existing content in the file, and update it with the following steps:
* using vim, follow below steps: Open the file with `vim index.js` and enter. Type`:` then type `%d` and enter. this will delete the entire content. Next, press `i` to enter the insert mode in vim. then, paste the entire code below in the file:

```bash
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
```
* Next, start the server using the command:
  ```bash
  node index.js
  ```
  <img width="831" height="156" alt="15-test-coniction" src="https://github.com/user-attachments/assets/0c5eb9dc-4cbf-48c4-a780-0ff5f442d103" />

* You shall see a message **Database connected successfully**, if so – we have our backend configured. Now we are going to test it.
  
##### Testing Backend Code without Frontend using RESTful API
 * Beacause we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code. We will use **Postman** to test our API.
* Create a **POST** request to the API
   ```bash
   http://<PublicIP-or-PublicDNS>:5000/api/todos
   ```
This request sends a new task to the To-Do list so the application could store it in the database. Also set header key **Content-Type** as **application/json**
 <img width="795" height="186" alt="16-" src="https://github.com/user-attachments/assets/69e67f83-4cff-45f4-8a02-873e754f02ab" />

### Step 2 Frontend Creation:

* Having completed the functionality of the backend, it is time to create a user interface for a Web client (browser) to interact with the application via API.
* In the same root directory as your backend code, which is the Todo directory, run:
  ```bash
  npx create-react-app client
  ```
  to create a new folder in your Todo directory called **client**
   <img width="448" height="65" alt="17- create-react-app client" src="https://github.com/user-attachments/assets/d6ced4f5-6984-48c5-b2a3-daaa56781623" />

##### Running a React App
* Before testing the react app, there are some dependencies that need to be installed: 
  * Install concurrently, used to run more than one command simultaneously from the same terminal window.
    ```bash
    npm install concurrently --save-dev
    ```
   <img width="529" height="247" alt="18-" src="https://github.com/user-attachments/assets/e2703979-f5eb-4c1a-b061-524d79b16f30" />
  
  * Install nodemon, used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.
    ```bash
    npm install nodemon --save-dev
    ```
  * In Todo folder open the **package.json** file. make changes to the **script** replace with the code below:
    
```bash
"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
```
  <img width="610" height="648" alt="19-" src="https://github.com/user-attachments/assets/04e2eef7-8faf-4918-9c15-a06f434077c7" />

##### Configure Proxy in package.json
* Change directory to **client**:
  ```bash
  cd client
   ```
* Open the package.json file:
  ```bash
  vi package.json
  ```
  
* Add the key value pair in the package.json file `"proxy":
   ```bash
   "http://localhost:5000"
   ```
 The purpose of this is to ensure access to the application directly from the browser by simply calling the server url like **http://localhost:5000** rather than always including the entire path like:
  ```bash
  http://localhost:5000/api/todos
  ```

* Navigate to the Todo directory and run: **npm run dev** The app should open and start running on localhost:3000
<img width="746" height="579" alt="20-" src="https://github.com/user-attachments/assets/f31f6e57-fc54-49fd-89bb-2f24346ec0bf" />

* To access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule.

##### Creating your React Components
* From your Todo directory run:
  ```bash
  cd client
  ```
* move to the src directory: `cd src`
* Inside your src folder create another folder called components:
   ```bash
   mkdir components
   ```
* Move into the components directory with:
  ```bash
  cd components
  ```
* Inside ‘components’ directory create three files **Input.js**, **ListTodo.js** and **Todo.js**:
   ```bash
   touch Input.js ListTodo.js Todo.js
   ```
* Open Input.js file:
  ```bash
  vi Input.js
  ```
* Paste the following: 
```bash
import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
```
 <img width="643" height="602" alt="21-input " src="https://github.com/user-attachments/assets/cfc735ca-1b44-4ac2-82ef-bbcb373bda0c" />
 
* Move back to the client folder :
   ```bash
   cd ../..
   ```
* In the client folder, install Axios:
   ```bash
   npm install axios
   ```
  <img width="563" height="183" alt="22-" src="https://github.com/user-attachments/assets/f8ff6e3b-bc62-4ce9-825e-3b1f1c0c9d67" />

* Next, go to components directory:
   ```bash
   cd src/components
   ```
* Then open your **ListTodo.js**:
  ```bash
  vi ListTodo.js
  ```
* Paste the following code into the ListTodo.js file:
   
```bash
import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
```
 <img width="518" height="371" alt="23-listfile" src="https://github.com/user-attachments/assets/e3debd47-d3c1-4c11-a687-bd669c52431e" />

* In in the Todo.js file you write the following code:
```bash
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
```
 <img width="644" height="599" alt="24-todo file" src="https://github.com/user-attachments/assets/3f0ede59-aea4-4e3a-8385-db5245f959f0" />

* Delete the logo and adjust our App.js. Navigate back to the **src** directory
* In the src folder run:
   ```bash
   vi App.js
   ```
* Copy and paste the code below into it
```bash
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
```
* Next, in the src directory open the App.css using:
  ```bash
  vi App.css
  ```
* Then paste the following code into App.css:
```bash
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
```
* Next, in the src directory open the index.css:
   ```bash
   vim index.css
   ```
* Copy and paste the code below:
```bash
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
```
* Go to the Todo directory:
  ```bash
  cd ../..
  ```
* When you are in the Todo directory run:
  ```bash
  npm run dev
  ```

* Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with all the functionality working: creating a task, deleting a task and viewing all your tasks.
 <img width="758" height="324" alt="27-the app" src="https://github.com/user-attachments/assets/95fe1b33-98d5-41a2-95d2-a22c9bbb1e87" />










  
