
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

8. Run `node index.js` to confirm that the server is working. If it is working propoerly, you should see a message like this; ![alt text](<Images/confirm index.js is running based on config file.png>)

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


## STEP 5: MONGODB DATABASE

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

6. Run the `node index.js` command again and you should see a 'Database connected successfully' message if your setup is correct.  ![alt text](<Images/mongoose DB working well with index.js command.png>). I had some challenges completing this and it is documented in my troubleshooting file for this task.

Now we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.
In this project, we will use Postman to test our API.








 