
# IMPLEMENT A WEB SOLUTION BASED ON MERN STACK IN THE CLOUD 

In this project, we are tasked to implement a web solution based on MERN stack in AWS Cloud.
MERN Web stack consists of following components:

* MongoDB - MongoDB is a popular, open-source, NoSQL (Not Only SQL) document database. This basically means that MongoDB uses a flexible document model. Data is stored in documents, which are similar to JSON (JavaScript Object Notation) objects. Data is stored in documents, which are similar to JSON (JavaScript Object Notation) objects.

* ExpressJS - ExpressJS often simply called Express, is a popular and flexible Node.js server-side web application framework.

* ReactJS - ReactJS often simply referred to as React, is a very popular, open-source JavaScript library used for building user interfaces (UIs). 

* Node.js - Node.js is a powerful and versatile runtime environment that allows developers to execute JavaScript code on a machine outside of a web browser.

### Prerequisites...
As a prerequisite to the course; 

* As usual, we will create an EC2 instance in the cloud, (refer to Project 1 for a guide with setting one up). Ensure to set a tag 'name' for the instance to be able to differentiate each one.

* For Windows users, we will be using Windows terminal to carry out the task in place of Git bash as we have used in previous tasks. Lets follow the following steps to set up.
    * You would already expectedly have windows terminal installed in your system. If you don't, download the windows terminal from the windows store and follow the installation prompts or use this [URL](https://learn.microsoft.com/en-us/windows/terminal/install)
    * You would also expectedly have Vscode installed in your system. If you don't, download VsCode using this [URL](https://code.visualstudio.com/download) select the Windows option and follow the prompts install correctly. Also install vscode extension like (material icon theme, Git extension pack and Bracket pair colorizer )
* We will then install the Open SSH server by running a series of commands. 
    - At the time of scripting this file and starting with Windows Server 2025, OpenSSH is now installed by default. But it is not installed by default for us so we need to install it with this command: `Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*' ` This returned the result for me below; ![alt text](<Images/package status for  open ssh.png>). As seen the SSH.Seerver is not installed for me.
    - Run the command below to install Open SSH server `Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`, then run `Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*' ` to confirm that it works as expected.
    - Run `ssh-keygen -t ecdsa` to generate a keypair. you will be prompted with the destination path of the key pair. For me it is `(C:\Users\DELL/.ssh/id_ecdsa):`

## TASK - DEPLOY A SIMPLE To-Do application that creates To-Do lists like this: 

### STEP 1 – BACKEND CONFIGURATION

1. Update and upgrade ubuntu libraries with these commands respoectively; `sudo apt update` and `sudo apt upgrade`

2. Get the location of Node js software from ubuntu repositories, using `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

3. Install Node.js and NPM with the command; `sudo apt-get install -y nodejs`.  **NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.** 

4. Use `node -v` to confirm that Nodejs was installed properly, and `npm -v` to confirm that the NPM package was installed successfully, it shows the version.

### STEP 2 – Appiication code setup

1. Create a new directory for the To-do project with `mkdir Todo`

2. Cd into the new directory and use the command `npm init` to initialise your project, so that a new file named *'package.json'* will be created. This file will normally contain information about your application and the dependencies that it needs to run. Follow the prompts after running the command. You can press Enter several times to accept default values, then accept to write out the package.json file by typing *'yes'*.

3. Run `ls` to confirm that the *package.json* was created.

4. **INSTALL EXPRESSjs** - To use Express we need to install in using npm, so run the command `npm install express`.

5. Create a file *index.js* with the command below
`touch index.js`. and run ls to confirm that it has been installed

6. Install the dotenv module with npm `npm install dotenv`

7. Open the index file using vim/vi i.e `vim index.js` then paste the code below; 
        
        const express = require('express');
        require('dotenv').config();
 
        const app = express();
 
        const port = process.env.PORT || 5000;
 
        app.use((req, res, next) => {
        res.header("Access-Control-Allow-Origin", "\*");
        res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
        next();
        });
 
        app.use((req, res, next) => {
        res.send('Welcome to Express');
        });
 
        app.listen(port, () => {
        console.log(`Server running on port ${port}`)
        });
exit insert mode with *escape*, save the file with *w* and exit vim with *qa* OR just use *'ZZ'* to exit from insert mode on vim.

8. Run `node index.js` to confirm that the server is working. If it is working propoerly, you should see a message like this;
 ![alt text](<Images/confirm index.js is running based on config file.png>)

9. The server has been configured to run on port 5000 so we need create an inbound rule to allow traffic on TCP port 5000, on our security groups for our instance. You should see a screenshot like this: ![alt text](<Images/Add port 5000 for nodejs.png>), then save the inbound rule 

10. Open up your browser and try to access your server’s Public IP or Public DNS name followed by port 5000; *http://<PublicIP-or-PublicDNS>:5000*. Your result should look like this;![alt text](<Images/we can access the server over the internet.png>).

## STEP 3: ROUTES

There are three actions that our To-Do application needs to be able to do:
 - Create a new task
 - Display list of all tasks
 - Delete a completed task
 
Each task will be associated with some particular endpoint and will use different standard [HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods): POST, GET, DELETE. For each task, we need to create routes that will define various endpoints that the *To-do* app will depend on. S

1. Let's create folder named *routes* `mkdir routes`
2. Cd into routes directory and create a file named *api.js*, using the command `touch api.js`
3. Open the *api.js* file and paste the command below;

        const express = require ('express');
        const router = express.Router();
 
        router.get('/todos', (req, res, next) => {
 
        });
 
        router.post('/todos', (req, res, next) => {
 
        });
 
        router.delete('/todos/:id', (req, res, next) => {
 
        })
 
        module.exports = router;

## STEP 4: MODELS  

Since the app is going to make use of Mongodb which is a NoSQL database, we need to create *a model*. A model is at the heart of JavaScript based applications, and it is what makes it interactive. We will also use models to define the *'database schema'* . This is important so that we will be able to define the fields stored in each Mongodb document. In essence, the *'Schema'* is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as *'virtual properties'*.

To create a Schema and a model, install [Mongoose](https://mongoosejs.com/) which is a Node.js package that makes working with mongodb easier. Lets follow the steps below to get it done.

1. Cd into the todo folder and run `npm install mongoose` to install Mongoose.

2. Create a new folder *'models'* with `mkdir models` and create a new file *'todo.js'* inside the models folder.

3. Run `vim todo.js` and paste the code below into it;
        
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

* Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model. In Routes directory, open api.js with `vim api.js`, delete the code inside with `:%d` command and paste there code below into it then save and exit;

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
 
        ZZmodule.exports = router;


## STEP 5: MongoDB DATABASE

We need a database where we will store our data. For this we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so you will need to sign up for a shared clusters free account, which is ideal for our use case. Sign up [here](https://www.mongodb.com/products/try-free/platform/atlas-signup-from-mlab) and follow the sign up process, select AWS as the cloud provider, and choose a region near you.

1. Complete the signup prompt and permit traffic from anywhere. You should have a similar result to what is in the screenshot below ![alt text](<Images/MongoDB setup (permit traffic from anywhere.png>).

2. Create a MongoDB database and collection inside mLab
![alt text](<Images/create mongoDB and collection inside mlab.png>)

3. In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

4. Create a file in your Todo directory and name it 
'.env'. `touch .env`. Add this connection string as code inside the *'.env'* file `DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`. However this is just a pattern as the correct username, password, dbname and network are missing. So we need to update those details using the correct information from our mongo DB. See the screenshot below to get the correct details from mongoDB: ![alt text](<Images/Get connection details for mongoDB_mongoose.png>).

5. Now we will update the *'index.js'* file to reflect the use of *'.env'* so that Node.js can connect to the database. Simply delete existing content in the file, and update it with the entire code below. To do that using vim, follow below steps;

     - Open the file with `vim index.js`
     - Press `esc `
     - Type `:%d` and hit *'enter'* (the content of the file will be deleted after this command),
     - Press `i` to enter insert mode again
     - Now paste the code below

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

6. Run the `node index.js` command again and you should see a 'Database connected successfully' message if your setup is correct.  ![alt text](<Images/mongoose DB working well with index.js command.png>) I had some challenges completing this and it is documented in my troubleshooting file for this task.

Now we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.
In this project, we will use Postman to test our API. [Install postman](https://www.getpostman.com/downloads/) and reference a guide to getting started with postman [here](https://www.youtube.com/watch?v=FjgYtQK_zLE).

7. Open Postman, create a POST request to the API *http://<PublicIP-or-PublicDNS>:5000/api/todos*. This request sends a new task to the To-Do list so the application could store it in the database.
Note: make sure your set header key `Content-Type` as `application/json`. You should have a feedback similar to this: ![alt text](<Images/postman post request to todo list .png>)

    P.S: Note that while running this command the `node.js` command needs to be running on your instance else the connection will be denied.

8. Create a GET request to your API on *http://<PublicIP-or-PublicDNS>:5000/api/todos*. This request retrieves all existing records from out To-do application (backend requests these records from the database and sends it us back as a response to GET request). You should have a result similar to this; 
![alt text](<Images/api get request.png>)


## STEP 6: FRONTEND CREATION 

Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the `create-react-app` command to scaffold our app.

In the same root directory as your backend code, which is the Todo directory, run: `npx create-react-app client`

### Running a React App

Before testing the react app, there are some dependencies that need to be installed.

1. Install **concurrently**. It is used to run more than one command simultaneously from the same terminal window. Use `npm install concurrently --save-dev`

2. Install **nodemon**. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes. Use `npm install nodemon --save-dev`

3. In Todo folder open the *package.json* file. Change the "scripts" part of it to the code below;

        "scripts": {
        "start": "node index.js",
        "start-watch": "nodemon index.js",
        "dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
        },

4. cd to client directory, open the *package.json* file and add the key value pair in the package.json file `"proxy": "http://localhost:5000"`.

    N.B: The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

5. cd into the 'Todo' directory and run `npm run dev`. you should get your app running **on localhost:3000** ie http://<13.53.39.222>:3000/ . Screenshot below; ![alt text](<Images/react frontend working on port 3000.png>)

   
    N.B  To be able to access the application from the Internet you have to open TCP port 3000 on EC2 by adding a new Security Group rule.![alt text](<Images/inbound rule 3000.png>).


### CREATING REACT COMPONENTS

1. create a folder named ***components* insider the *'src'* folder in *'client'* folder. Use the command `mkdir client/src/components`

2. create 3 files inside the components directory. `touch Input.js ListTodo.js Todo.js`

3. open the *input.js* file paste the following code into it 

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

To make use of [Axios](https://github.com/axios/axios), which is a Promise based HTTP client for the browser and node.js, you need to cd into your client from your terminal and run `yarn add axios` or `npm install axios`.

4. cd to the client folder and install axios; `cd client` and `npm install axios`. 

n.b I experienced challenge doing this which is documented in my troubleshooting file.

#### FRONTEND CREATION (CONTINUED)

5. cd into the 'components' directory from the client directory: `cd src/components`

6. paste the code below into the *'ListTodo.js'* file.

        `vi ListTodo.js` 

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

7. paste the code below into the *'Todo.js'* file.


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


We now need to make little adjustment to our react code. Delete the logo and adjust our App.js file.

8. Now cd to the *'src'* folder in the client folder - `cd ..` run `vi App.js ` to edit the file and paste the code below inside it;


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


9.  Exit the editor and open *'app.css'* file - `vi App.css`, then paste the code below into it.


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

10. Exit the editor on the App.css file, then open the index.css file with `vi index.css` and paste the code below into it.

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


11. Go to the Todo directory with `cd ../..` and when you are in the Todo directory, run `npm run dev` and makesure that the package compiles succesfully, then try access the URL 'http://13.53.39.222/:3000/' from your browser and you should get a response similar to the one below; ![alt text](<Images/final server output for Mern stack.png>).

If you got to this stage, then you have completed this task by making a simple To-Do and deployed it to MERN stack. You wrote a frontend application using React.js that communicates with a backend application written using Expressjs. You also created a MongoDB backend for storing tasks in a database.







 