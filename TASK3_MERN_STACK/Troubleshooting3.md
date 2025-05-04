
## Challenge with connecting to Mongoose DB 

I experienced challenges with connecting to the mongoose DB I created for a coupld of reasons;

- I copied the connectin string from the setup prompts; `DB = 'mongodb+srv://User01:<db_password>@emma-mongo.tkxsfmb.mongodb.net/?retryWrites=true&w=majority&appName=Emma-Mongo'` and in trying to replace <db_password> with the correct password, I kept typing the actual password into the anchor brackets like this <actual_password> instead of using '...:actual_password@...'. I was getting 'bad authentication error' when trying to run index file to connect to the database. 

- I discovered and corrected this this error, however I ran into sonnection string error and this was because I did not defind the parameter.  ![alt text](<Images/Mongoose DB connection string error.png>).

- I defined the parameter correctly in single quotes and the connection worked.


# POSTMAN 
Click [HERE](https://www.youtube.com/watch?v=FjgYtQK_zLE) to learn how perform [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operartions on Postman



## Issues with completing Running a React App' task.

In trying to execute the Running a React App' task I experienced some of the underlisted issues.

After installing [concurrently](https://www.npmjs.com/package/concurrently) with `npm install concurrently --save-dev` and [nodemon](https://www.npmjs.com/package/nodemon) with `npm install nodemon --save-dev` and adding the key value pair `"proxy": "http://localhost:5000"` to the package.json file in the clients folder. The `npm run dev` command that is supposed to launch the application was not running and was throwing an error with the addition of the key value pair and this was because I missed a comma while adding the value pair to the package.json file. 

After that I ran `npm run dev` again and get the error below;  ![alt text](<../TASK3_MERN_STACK/Images/npm run dev init error.png>)

This was because of the package version which was an older version (16) than the version of Node that I was running (18). I was then required to install nvm to eable to downgrade the package, with he following commands;

        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

        export NVM_DIR="$HOME/.nvm"[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

        source ~/.bashrc

        nvm use 16

Then I ran `nvm install 16` to downgrade the node package and executed `nvm run dev` again which failed with another error below; ![alt text](<../TASK3_MERN_STACK/Images/npm run dev app.js not working.png>)

This indicated a missing line in the app.js file and I corrected this by adding a line `import React from 'react';` to the app.js file which then permitted to file to compile successfully: ![alt text](<../TASK3_MERN_STACK/Images/Import react to app.js.png>).

This allowed me get the desired result of the app front end running on localhost:3000 ![alt text](<../TASK3_MERN_STACK/Images/react frontend working on port 3000.png>)

## Issues with installing Axios

In trying to install [axios](https://github.com/axios/axios) `npm install axios`, I got the error below:
![alt text](<Images/npm install axios node version issue.png>) This relates to the node version and to fix this, I installed the newer version of nvm with `nvm install 18` then `nvm use 18`.

Then I ran the npm install axios again and it installed axios.


## Issues with displaying the fianl result of the reach app.

At the end of running through all the tasks my result was not consistent with the expected final result of the task in the sense that it was not rendering the correct information to the front end app.![alt text](<Images/final server output for Mern stack.png>)

I troubleshot with my tutor and we discovered the following issues;

- Bad copying of configuration files - I did not copy the configuration files correctly, I missed out the ending semi-colons on the configuration scripts. This happened on my `input.js` file as well as `ListTodo.js` files. Also on my `App.css' file, I pasted the configuration script multiple times. I had multiple occurences of App, input and button scripts

- Multiple `Input.js` files - I wrongly created an extra empty input.js file. 