# DEPLOY A LEMP STACK WEBSITE IN AWS CLOUD

LEMP is an open-source web application stack used to develop web applications. The LEMP stack is a combination of four open-source technologies that are used in web development. LEMP Stands For:

* L- Linux Operating System - The operating system that runs the web server.
* E- Nginx Server -  The web server software that handles HTTP requests.
* M- MySQL Database - The relational database management system that stores the website’s data.
* P- PHP - The programming language used to build dynamic web applications.

LEMP enjoys good community support and is used around the world in many highly-scaled web applications. Nginx is the second most widely used web server in the world following Apache.

To complete this project, we will need a new EC2 instance as we set up in the last task, please refer to this video [here](https://www.youtube.com/watch?v=xxKuB9kJoYM&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=6) for a guide.

## STEP 1: INSTALL NGINX 

NGINX is an open-source web server software used for reverse proxy, load balancing, and caching. It provides HTTPS server capabilities and is mainly designed for maximum performance and stability. It also functions as a proxy server for email communications protocols, such as IMAP, POP3, and SMTP. Lets run through the following steps to install Nginx.

* Update the apt package library and install nginx - `sudo apt update && sudo apt install nginx`. Click 'y' when prompted if you want to install nginx
* Confirm if Nginx was successfully installed with `sudo systemctl status nginx`. You should a text 'active (running)' if it was successfully installed.
* To receive traffic, we need to edit the inbound rules and enable HTTP traffic on port 80, from anywhere i.e 0.0.0.0:0 I am utilizing the same security group i used in task 1, so i already have this set up.
* If the installation is complete, try call Nginx internally from our server by running a curl command `curl http://localhost:80` or `curl http://127.0.0.1:80`. Both will return the same result, but the difference is the approach/command. In the first case we try to access our server via DNS name and in the second one – by IP address (in this case IP address 127.0.0.1 corresponds to DNS name ‘localhost’ and the process of converting a DNS name to IP address is called "resolution"). 
    * All web servers use port by default so sometimes, if you do not specify the port 80 (:80) the request will still run successfully.


## STEP 2: INSTALL MYSQL

* Update the apt package library and install mysql-server - `sudo apt update && sudo apt install mysql-server`. Click 'y' when prompted if you want to install mysql-server.
* Run `sudo systemctl status mysql` to confirm the status of the server. 
* Run `sudo mysql` to connect to the msql server feedback you get should look something like this ![alt text](<Images/mysql confirmation page .png>)
* Set a password for mysql root user by running the command ` ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY ('your password');`. 
* Exit the mysql shell with `exit` command 
* Run a security script that comes pre-installed with MySQL; `$ sudo mysql_secure_installation` This script will remove some insecure default settings and lock down access to your database system. 
    * Enabling this feature is like a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
    * If you answer “yes”, you’ll be asked to select a level of password validation. Remember that if you enter 2 for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words e.g PassWord.1.
 * Attempt logging into mysql DB with `sudo mysql`. You will be denied access as you have already set up a password. To bypass this run `sudo mysql -p`. This will prompt you to input a password, then you can gain access to your mysql server.


 ## STEP 3: INSTALL PHP

 We will install PHP to process code and generate dynamic content for the web server. While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. We’ll need to install *php-fpm*, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, you’ll need *php-mysql*, a PHP module that allows PHP to communicate with MySQL-based databases. Core PHP packages will automatically be installed as dependencies
 
 * Install *php-fpm* and *php-mysql* together with the command `sudo apt install php-fpm php-mysql`.

## STEP 4: CONFIGURE NGINX TO USE PHP PROCESSOR

When using the Nginx web server, you can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use projectLEMP as an example domain name.

On Ubuntu,  Nginx has one server block enabled by default and is configured to serve documents out of a directory at `/var/www/html`. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instead of modifying `/var/www/html`, we’ll create a directory structure within /var/www for your domain website, leaving `/var/www/html` in place as the default directory to be served if a client request does not match any other sites.

* Create a web directory for your domain with `sudo mkdir /var/www/projectLEMP`
* Assign ownership of the directory with the $USER environment variable, which will reference your current system user: `sudo chown -R $USER:$USER /var/www/projectLEMP`. You can confirm the change by running `ll /var/www/projectLEMP` and your feedback should be something like this ![alt text](<Images/chnge owndership for prjctlemp dir .png>)
* Open a new configuration file in Nginx’s sites-available directory using nano (or your preferred command line editor). `sudo nano /etc/nginx/sites-available/projectLEMP`
* This will create a new blank file, you can then paste the bare-bones configuration inside it:

        #/etc/nginx/sites-available/projectLEMP`
 
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
    	        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
 	            }
 
	            location ~ /\.ht {
    	        deny all;
	            }
 
        }

* Here’s what each of these directives and location blocks do:
    * listen — Defines what port Nginx will listen on. In this case, it will listen on port 80, the default port for HTTP.
    * root — Defines the document root where the files served by this website are stored.
    * index — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.
    * server_name — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server’s domain name or public IP address.
    * location / — The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.
    * location ~ \.php$ — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.
    * location ~ /\.ht — The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root, they will not be served to visitors.

* Save and close the file. If you’re using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.

* Activate the configuration by linking the config file from Nginx sites-enabled directory: `sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/` - This will tell Nginx to use the configuration next time it is reloaded.
* Test your configuration for syntax errors with the command: `sudo nginx -t`. You should get a result similar to this ![alt text](<Images/confirm nginx config wt not errors .png>). Rerun the same process if you get any errors.
* You also need to disable default Nginx host configured to listen on port 80. Run the command: `sudo unlink /etc/nginx/sites-enabled/default`. Reload nginx to apply the changes with the command: `sudo systemctl reload nginx`
* After completing this, your new website will be active, but the web root dir */var/www/projectLEMP* is till empty. We need to create an index.html file in that location to allow us test that the new server block works as expected. 

    ``sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html``
* You can then open the website URL using the IP address like this: `http://<Public-IP-Address>:80` or using the DNS name like this `http://<Public-IP-Address>:80`. For me, my ip address is: *13.60.15.18* and my public DNS is *ec2-13-60-15-18.eu-north-1.compute.amazonaws.com*. Bear in mind that this is a dynamic IP. You should have the same results from IP and DNS similar to the images below.

![alt text](<Images/nginx lemp + ip.png>)
![alt text](<Images/nginx lemp + DNS.png>)

Note: Leave this file in place as the landing page for your application until you set up an index.php file to replace it. Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.

## STEP 5: TEST PHP WITH NGINX 

After confirming that your LEMP STACK is set up, you have to test that Nginx is correctly handing out php files off to the PHP processor. 

* To do this, create a test php file called *index.php* within the document root with your text editor i.e nano
`sudo nano /var/www/projectLEMP/index.php`. Then paste the lines below into the new file . This is valid PHP code that will reveal information about your server.

        <?php

        phpinfo();
* Visit the domain name or public IP address set up in the nginx config file followed by '/info.php' i.e *http://ec2-13-60-15-18.eu-north-1.compute.amazonaws.com/index.php or http://13.60.15.18/index.php* and you should get a result similar to the image below.
![alt text](<Images/php config working .png>)

* Finally, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. Run the command to remove that file:`sudo rm /var/www/your_domain/info.php`
You can always regenerate this file if you need it later.

## STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)

In this step you will create a test database (DB) with simple "To do list" and configure access to it, so the Nginx website would be able to query data from the DB and display it.

At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support caching_sha2_authentication, the default authentication method for MySQL 8. We’ll need to create a new user with the mysql_native_password authentication method in order to be able to connect to the MySQL database from PHP.

* Login to your mysql console using the root account: `sudo mysql` or `sudo mysql -p` if you set up a password when you configured mysql. You should see a prompt like this: **mysql>** if you are correctly logged in.

* Create a database called **example_db** with the command:

            CREATE DATABASE `example_db`;

* Create a new user and grant full privileges on the database you have just created. The following command creates a new user named 'sample_user', using mysql_native_password as default authentication method. We’re defining this user’s password as 'sample_user':

        CREATE USER 'sample_user'@'%' IDENTIFIED WITH mysql_native_password BY 'Password_1';

* Give this user permission over the 'example_db' database: `GRANT ALL ON example_db.* TO 'sample_user'@'%';` This gives **sample_user** full privileges over **example_db** but restricts him from creating or modifying other databases on the server.
* Exit mysql shell with  the `exit` command then try login for the new user with `mysql -u sample_user -p`
* You should see a **mysql>** prompt if you are successfully logged in. You can then run a query like `SHOW DATABASES;` to see datases on server. You should see an image like this;![alt text](<Images/sample_user login.png>)
* Next, create table named *todo_list* with the commands below:

        i. USE example_db;
        ii. CREATE TABLE example_database.todo_list (
 	        item_id INT AUTO_INCREMENT,
 	        content VARCHAR(255),
 	        PRIMARY KEY(item_id)
            );
        iii. INSERT INTO todo_list (content)VALUES        ("Wake up"); 
        INSERT INTO todo_list (content) VALUES ("Pray to God");
        INSERT INTO todo_list (content) VALUES ("Take what's mine");
        INSERT INTO todo_list (content) VALUES ("Take some more");
        INSERT INTO todo_list (content) VALUES ("Give God his share"); 
        INSERT INTO todo_list (content) VALUES ("Take Caesar's own");

* Run the query `select * from todo_list` and your output should looks something like the image below: ![alt text](<Images/mysql create db + table.png>)
* Exit the mysql shell.

Now you can create a PHP script that will connect to MySQL and query for your content. 

* Create a new PHP file in your custom web root directory using your preferred editor. We’ll use vi for that: 
`nano /var/www/projectLEMP/todo_list.php`
* Paste the config below inside of the todo_list.php file:

        <?php
        $user = "sample_user";
        $password = "Password_1";
        $database = "example_db";
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

* Save and exit the file with `ZZ` command.
* You can now access the page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:
http://<Public_domain_or_IP>/todo_list.php and you should have a result similar to the image below;
![alt text](<Images/php calling mysql.png>). If you have the same result, your setup is working correctly.









