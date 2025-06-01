## Understanding Client-Server Architecture

At its core, client-server architecture is a distributed application structure that partitions tasks or workloads between the providers of a resource or service, called **servers**, and service requesters, called **clients**. It further refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another. See image below:

![alt text](<Project5_Images/Screenshot 2025-05-31 231201.png>)

**CLIENTS** - They request services or resources from the server. Examples are: web browser (Chrome, Firefox), an email client (Outlook, Gmail app), a mobile app (Facebook, Instagram), or even a desktop application that needs to access data from a central source.

**SERVERS** - They provide services or resources to clients. They "serve" requests. Examples are web servers (Apache, Nginx) serving web pages, a database server (MySQL, PostgreSQL) storing and retrieving data, an email server (Exchange, Postfix) handling emails, a file server, an application server, etc.

In this task, we will be implementing a client server architecture using mysql database management system.

## Implement a Client-Server Architecture using MySQL Database Management System (DBMS).

- Step 1 - Create and configure two Linux-based virtual servers (EC2 instances in AWS). please refer to the video [here](https://www.youtube.com/watch?v=xxKuB9kJoYM&list=PLtPuNR8I4TvkwU7Zu0l0G_uwtSUXLckvh&index=6) for a guide. 

    ``` 
    Name Server A - `mysql server`
    Name Server B  - `mysql client`
    ```
- Step 2 - Update apt repository and Install MySQL Server software on *'Mysql server'* - `sudo apt update` or `sudo apt-get update` & `sudo apt install mysql-server`. 

    Further confirm that he service is up and running - `sudo systemctl status mysql`

- Step 3 - Install MySQL client software on *'Mysql client'*. Follow the steps below;

    - Create a folder - `mkdir Downloadss`
    - Add the MySQL repository by downloading the release package with 'wget' into *Downloadss*, Use [guide](https://dev.mysql.com/doc/mysql-apt-repo-quick-guide/en/#apt-repo-setup): `wget http://dev.mysql.com/get/mysql-apt-config_0.8.34-1_all.deb`
    -  Install the downloaded package: `sudo dpkg -i mysql-apt-config_0.8.23-1_all.deb`
    - Update package list: `sudo apt update` or `sudo apt-get update`.
    - Install the MySQL server: `sudo apt install mysql-server`
    - Check the server's status: `sudo systemctl status mysql`. You should have a feedback showing an *active(running)* status. ![alt text](<Project5_Images/myssql installed and running.png>)
    - Install mysql client: `sudo apt install mysql-client`.
    - Confirm that you have installed the client successfully with the command: `mysql --version`. I had delays moving forward because I kept running the command: `sudo systemctl status mysql-client` and getting the error *Unit mysql-client.service could not be found.*

### Step 4 - Allow the two servers communicate with themselves. Do this with the following steps.

    - By default, both EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. Use *"mysql server's"* local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so we will have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, do not allow all IP addresses to reach your ‘mysql server’ – allow access only to the specific local IP address of  ‘mysql client’. ![alt text](<Project5_Images/add mysql tcp port on mysql server.png>). 
    Confirm your hostname with: `hostname -i`

    - Configure MySQL server to allow connections from remote hosts. Modify the file `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf` and change 
    ```
    bind-address            = 127.0.0.1
    mysqlx-bind-address     = 127.0.0.1

    TO 

    bind-address            = 0.0.0.0
    mysqlx-bind-address     = 0.0.0.0
    ```.


    -  Restart mysql server - `sudo systemctl restart mysql`.

- Step 5 - From *mysql client* Linux Server connect remotely to *mysql server* Database Engine without using SSH. Ensure to use the mysql utility to perform this action. To do this;

    - Create a user for mysql client server in the DB engine. login to mysql: `sudo mysql` and grant privileges: `CREATE USER 'admin'@'172.31.31.172' IDENTIFIED BY '1111';`. You should get a response of `Query OK, 0 rows affected (0.06 sec)`.

    - Attempt to access *mysql server* again from *mysql client* with `mysql -u admin -h 172.31.23.202 -p` and inputted the password as requested, then I gained access to *mysql server* DB engine to run commands as I got the mysql prompt like this ![alt text](<Project5_Images/access mysql server from client.png>).
 
- Step 6 - Finally confirm that you can execute commands on mysql server. Run the command `show databases` and you should be able to see the feedback from that query. something like this; ![alt text](<Project5_Images/can run command on server from client.png>).

If you can see this then you have deployed a fully functional MySQL Client-Server set up.

    







