# DEPLOY A LAMP STACK WEBSITE IN AWS CLOUD 

A "LAMP stack" refers to a popular open-source software combination used for building websites and web applications, 
where "LAMP" stands for Linux (operating system), Apache (web server), MySQL (database), and PHP (programming language); 
essentially, it's a group of technologies that work together to host and deliver dynamic web content. 

* Components breakdown  

    - **Linux:** The operating system that provides a stable foundation for the entire stack. 
    - **Apache:** The web server responsible for handling HTTP requests and delivering web content. 
    - **MySQL:** The relational database management system used to store and manage application data. 
    - **PHP:** The scripting language used to generate dynamic content on the server-side. 



## STEP 1: SET UP YOUR EC2 INSTANCE IN AWS CLOUD

AWS can provide you with a free virtual server called an EC2 instance. Follow  the instruction in the video [here](https://www.youtube.com/watch?v=xxKuB9kJoYM&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=6) to achieve same. 
Be sure to use a linux based AMI for your EC2 instance.

Connect to your EC2 instance with the video guide [here ](https://www.youtube.com/watch?v=TxT6PNJts-s&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=7).
You will be required to SSH into your virtual server as such you will require a tool called PUTTY that utilizes the SSH protocol to establish connectivity into the server. I used a different approach to
achieve this, this is contained in my troubleshoot file [here](https://github.com/4eyedZedddevops_task_1/blob/934bb3c66efc3466e851b072ea0138173b52025f/TROUBLESHOOTING).


## STEP 2: INSTALL APACHE AND UPDATE THE EC2 INSTANCE FIRE WALL

Apache HTTP Server is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free. 
It runs on 67% of all webservers in the world. It is fast, reliable, and secure. It can be highly customized to meet the needs of many different environments by using extensions and modules. 
Most WordPress hosting providers use Apache as their web server software. However, websites and other applications can run on other web server software as well. Such as Nginx, Microsoft’s IIS, etc.
Download it from the web [url](https://httpd.apache.org/).

* Update your Linux lib and install Apache2 with the command  `sudo apt update && sudo apt install apache2`
* Verify that it's running on the OS with the command `sudo systemctl status apache2`
*  Enable inbound traffic to the web server by editing the inbound rules under the *security groups* menu on your EC2 instance set up. Add a new rule for *HTTP* traffic from anywhere *IP (0.0.0.0)* on *TCP port 80*.
* Run the command `curl` <http://127.0.0.1:80> to access the server on ubuntu shell. This will produce some strangely formatted text, this just means that Apache web service responds to ‘curl’ command with some payload. 
* To see if our web server is responding, run the url http://<Public-IP-Address>:80 . Pass your public IPV4 address of your instance to the bracket. if you can see apache on your page, then its working well.


## STEP 3: INSTALL MYSQL 

 MySQL is a popular relational database management system used within PHP environments.
 
* Update your Linux lib and install Mysql server with the command  `sudo apt update && sudo apt install mysql-server`
* Run a security script that comes pre-installed with MySQL, to remove some insecure default settings and lock down access to the database system `sudo mysql_secure_installation`.
    This will ask if you want to configure the **VALIDATE PASSWORD COMPONENT**. Enabling this feature is something of a judgment call. If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
* Test if you can login to Mysql console by running `sudo mysql`


## STEP 4: INSTALL PHP 
PHP is the component of our setup that will process code to display dynamic content to the end user. 
In addition to the php package, you’ll need *php-mysql*, a PHP module that allows PHP to communicate with MySQL-based databases. 
You’ll also need *libapache2-mod-php* to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

* Install all 3 packages by running `sudo apt install php libapache2-mod-php php-mysql`
* To confirm the PHP version `php -v`


## STEP 5 ++ CREATE A VIRTUAL HOST FOR THE WEBSITE USING APACHE 

In this project, we will set up a domain called **'projectlamp'**, but you can replace this with any domain of your choice.

* Create the directory for projectlamp `sudo mkdir /var/www/projectlamp`
* Assign ownership of the directory with the $USER environment variable, which will reference the current system user `sudo chown -R $USER:$USER /var/www/projectlamp`
* Create and open a new configuration file in Apache’s 'sites-available' directory using vi or vim (cmd line editor) `sudo vi /etc/apache2/sites-available/projectlamp.conf`
* Paste in the following bare-bones configuration by hitting on *'i'* on the keyboard to enter the insert mode, and paste the text:
    - <VirtualHost *:80>
        - ServerName projectlamp
        - ServerAlias www.projectlamp 
        - ServerAdmin webmaster@localhost
        - DocumentRoot /var/www/projectlamp
        - ErrorLog ${APACHE_LOG_DIR}/error.log
        - CustomLog ${APACHE_LOG_DIR}/access.log combined
    - </VirtualHost *>
* Save the file and exit with these 3 commands - 'escape' + ':' + 'wq' + 'enter".
* Show the new file in the sites-available directory `sudo ls /etc/apache2/sites-available`. **You will see something like this:** (*000-default.conf*  *default-ssl.conf*  *projectlamp.conf*)
* With this VirtualHost configuration, Apache will serve **'projectlamp'** using */var/www/projectlampl* as its web root directory. 
* If you would like to test Apache without a domain name, you can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines.
* Use a2ensite command to enable the new virtual host `sudo a2ensite projectlamp`
* You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, 
because in this case Apache’s default configuration would overwrite your virtual host.
* Use a2dissite command to disable Apache’s default website `sudo a2dissite 000-default`
* To make sure your configuration file doesn’t contain syntax errors, run - `sudo apache2ctl configtest`
* Reload Apache so these changes take effect with - `sudo systemctl reload apache2`
* Your new website is now active, but the web root */var/www/projectlamp* is still empty. 
* Create an *index.html* file in that location to test that the virtual host works as expected - `sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`
* Open your website URL using IP address - *http://(Public-IP-Address):80* or *http://(Public-DNS-Name):80*. 
   You should see the text from ‘echo’ command you wrote to index.html file, it means your Apache virtual host is working as expected. 
   In the output you will see your server’s public hostname (DNS name) and public IP address. 
* You can leave this file in place as a temporary landing page for your application until you set up an index.php file to replace it. 
   Once you do that, remember to remove or rename the index.html file from your document root, as it would take precedence over an index.php file by default.

## STEP 6 ++ ENABLE PHP on the website 

* With the default **DirectoryIndex** settings on Apache, a file named *index.html* will always take precedence over an *index.php* file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.
* If you choose to change this behavior,  you’ll need to edit the */etc/apache2/mods-enabled/dir.conf* file and change the order in which the index.php file is listed within the DirectoryIndex directive. To do this, run `sudo vim /etc/apache2/mods-enabled/dir.conf`
    - \<IfModule mod_dir.c>
        - **#Change this:**
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        - **#To this:**
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
    - \</IfModule>
* For the changes to take effect, close the file and reload Apache. Run `sudo systemctl reload apache2`.
* Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files. Create a new file named *index.php* inside your custom web root folder: ` sudo vi /var/www/projectlamp/index.php`
* This will open a blank file. Add the following text, which is valid PHP code, inside the file 
    - \<?php
    - phpinfo();
* When you are finished, save and close the file, refresh the page and you will see a page similar to ![this:]("C:\Users\DELL\Pictures\Screenshots\Screenshot 2025-03-16 095439.png")
* This page provides information about your server from the perspective of PHP. It is useful for debugging and to ensure that your settings are being applied correctly.
If you can see this page in your browser, then your PHP installation is working as expected.


