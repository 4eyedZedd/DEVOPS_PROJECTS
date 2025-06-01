## Common troubleshooting issues I experienced;

1.  To confirm that you have installed the client successfully with the command: `mysql --version` and the expected feedback is `mysql  Ver 8.0.42-0ubuntu0.24.04.1 for Linux on x86_64 ((Ubuntu))`. I had delays moving forward because I kept running the command: `sudo systemctl status mysql-client` and getting the error:
``` 
Unit mysql-client.service could not be found.
``` 
I had the same scenario with `sudo systemctl status mysql-server`


2. Configuring MySQL server to allow connections from remote hosts

- While configuring MySQL server to allow connections from remote hosts. Modify the file `sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf` and change 
    ```
    bind-address            = 127.0.0.1
    mysqlx-bind-address     = 127.0.0.1

    TO 

    bind-address            = 0.0.0.0
    mysqlx-bind-address     = 0.0.0.0

At this point I experienced error modifying the file `E505: "mysqld.cnf" is read-only (add ! to override)` because I did not open the file with sudo. I corrected that.

I also got the error `ERROR 2003 (HY000): Can't connect to MySQL server on '172.31.23.202:3306' (111)` while trying to connect the to server and that was because I did not restart mysql server after modifying the configuration file. I ran the command `sudo ss -tulnp | grep mysqld` to discover that then did `sudo systemctl restart mysql ` to correct it.


3.     - Create a user for mysql client server in the DB engine. login to mysql: `sudo mysql` and grant privileges 

At this point, I was getting the error `ERROR 1130 (HY000): Host 'ip-172-31-31-172.eu-north-1.compute.internal' is not allowed to connect to this MySQL server` while running the command `mysql -u mysql -p 1111 -h 172.31.23.202` and the command `mysql -u mysql -h 172.31.23.202` on my *mysql client server*

While creating the user and granting privileges, with the command ` GRANT ALL PRIVILEGES ON *.* TO 'root'@'
    '> ip-172-31-31-172.eu-north-1.compute.internal' IDENTIFIED BY '1111';`, I got the error: 
    `ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'ERROR 1064 (42000): You have an error in your SQL syntax' at line 1` and this was because I was using an outdated command that was for mysql versions before v8.

I corrected the query to `CREATE USER 'admin'@'172.31.31.172' IDENTIFIED BY '1111';` and this executed successfully. with the response `Query OK, 0 rows affected (0.06 sec)`


4 - Connect to *mysql server* from *mysql client*: `mysql -u admin -h 172.31.23.202 -p`

At this point I kept experiencing the error `ERROR 1045 (28000): Access denied for user 'admin'@'ip-172-31-31-172.eu-north-1.compute.internal' (using password: NO)` when I ran the commands 
    
    ```
    sudo mysql
    mysql -u admin -h 172.31.23.202
    mysql -u mysql -h 172.31.23.202
    ```


These happened because I tried to access the server without inputting passwords for users 'admin' and 'mysql'.
    
I also got the error `ERROR 1044 (42000): Access denied for user 'admin'@'172.31.31.172' to database '1111'` while running the command `mysql -u admin -h 172.31.23.202 -p 1111` This happened because I referenced the db as `-p 1111`

I then  ran the command `mysql -u admin -h 172.31.23.202 -p` and inputted the password as requested, then I gained access to *mysql server* DB engine to run commands as I got the mysql prompt


