

## .PHP Config not working

I experienced challenges while trying to run the urls
``` 
http://13.60.15.18/index.php and http://ec2-13-60-15-18.eu-north-1.compute.amazonaws.com/index.php
```
and I was getting a '502' bad gateway error. While troubleshooting, I followed thefollowing steps

* Review the configuration file and prioritize index.php files over index.html
* Changed the servername on the configuration file to my instance public ipv4 and DNS name
* I unlinked sites-available default file again.
* I reloaded nginx and php-fpm again.
* I pinged the server to know if it was alive
* 

Although all these did not solve the problem, but it gave me an insight into troubleshooting issues more.

* I then escalated to my tutor and we discovered that the error was because of a version problem with the configuration file for nginx.

I had downloaded a php-fpm version 8.3 and while the configuration file I used had a version 8.1, after I corrected it, the script ran as expected.


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





