
# WEB STACK IMPLEMENTATION (LEMP STACK)

### SSH into my Nginx Web Server

`ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>`

![Alt text](<Images/ssh into the Nginx Web Server 1.PNG>)

![Alt text](<Images/ssh into the Nginx Web Server.PNG>)

## Installing the Nginx Web Server

`sudo apt update`

![Alt text](<Images/sudo apt update.PNG>)

`sudo apt install nginx`

![Alt text](<Images/sudo apt install nginx.PNG>)

To verify that nginx was installed properly

`sudo systemctl status nginx`

![Alt text](<Images/Verify nginx was installed properly.PNG>)

Before we can receive any traffic by our webserver, we need to open TCP port 80, which is the default port that our web browsers use to access web pages on the internet.

We have the TCP port 22 that opens by default on our EC2 machine to access it via the SSH, however, we need to add a rule to EC2 configuration to open inbound connection through port 80.

Open TCP port 80, which is the default web port for web browswers to access a web page

![Alt text](<Images/Open TCP Port 80.PNG>)

- To access Nginx server via local machine

Run curl http://localhost:80

or curl http://127.0.0.1:80

![Alt text](<Images/Access Nginx Locally via shell.PNG>)

To Access Nginx via the web browser using the public IP Address.

`http://<Public-IP-Address>:80`

![Alt text](<Images/Access Nginx via the web using public IP.PNG>)

To retrieve or confirm my public IP using the shell script

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

![Alt text](<Images/To confirm my public IP via the shell.PNG>)

# INSTALLING MYSQL

MySQL is an open-source relational database management system and is the third layer of the LAMP stack. The LAMP model uses MySQL for storing, managing, and querying information in relational databases. For example, developers store application data, such as customer records, sales, and inventories. When a user searches for information, the web server queries the stored data in MySQL. Query refers to special instructions for manipulating data in a relational database with the SQL language.

To Install mysql on Nginx Server

`sudo apt install mysql-server`

![Alt text](<Images/sudo apt install mysql.PNG>)

To Login into mysql console

`sudo mysql`

![Alt text](<Images/sudo mysql.PNG>)

A Quick Note: It is recommended to run a security script that comes pre-installed with MySQL. This script will remove insecure default settings and lock down access to the database. However, before this can be done, we will have to set a password for the root user, using mysql_native_password as default authentication method. We would define the user's password as *PassWord.1*

- To set password for the Root user
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

![Alt text](<Images/Alter the root user password in MySQL.PNG>)

Exit MySQL

`exit`

![Alt text](<Images/exit MySQL.PNG>)

Start the interactive script my running the below command

`sudo mysql_secure_installation`

![Alt text](<Images/sudo mysql secure installation.PNG>)

- To Confirm password change for Mysql root user and change anonymous user

![Alt text](<Images/To change MySQL Password.PNG>)

To Complete the installation

![Alt text](<Images/To complete the installation.PNG>)

To Login to MySQL Console

`sudo mysql -p`

![Alt text](<Images/To Login to MySQL Console.PNG>)

Exit from MySQL

`exit`

![Alt text](<Images/Exit from MySQL.PNG>)

# INSTALLING PHP

Nginx is installed to serve your content while MySQL is installed to store and manage our data. Hence, PHP is used to process code and generate dynamic content for the web server.

While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP based website, but it requires additional configuration. Therefore, we will need to install *php-fpm* which stands for "PHP fastCGI process manager", and tell Nginx to pass PHP requests to this software for processing. 

Also, *php-mysql*, a PHP module that allows PHP to communicate with MySQL-based databases.

To Install the 2 packages.

`sudo apt install php-fpm php-mysql`

![Alt text](<Images/sudo apt install php-fpm php-mysql.PNG>)

# Configuring Nginx to Use PHP Processor

When using the Nginx web server, we can create server blocks (similar to virtual hosts in Apache) to encapsulate configuration details and host more than one domain on a single server. In this guide, we will use *projectLEMP* as an example domain name.

On Ubuntu, Nginx has one server block enabled by default and is configured to serve documents out of a directory at */var/www/html*. While this works well for a single site, it can become difficult to manage if you're hosting multiple sites. Instead of modifying */var/www/html*, we'll create a directory structure within */var/www* for the *your_domain* website, leaving */var/www/html* in place as default directory to be served if a client request does not match any other sites.

Create a root web directory for *your_domain* as follows

`sudo mkdir /var/www/projectLEMP`

To cd into the root directory

`sudo -i`

`cd /var/www/`

![Alt text](<Images/cd to the root directory.PNG>)

Create a folder called *projectLEMP*

`sudo mkdir /var/www/projectLEMP`

![Alt text](<Images/sudo mkdir dir projectLEMP.PNG>)

Assign ownership of the directory to the $USER environment variable, which will reference the current system user

`sudo chown -R $USER:$USER /var/www/projectLEMP`

![Alt text](<Images/Assign ownership in the projectLEMP directory.PNG>)

Then, create and open a new configuration file in Nginx *site-available* directory using the *nano* command.

`sudo nano /etc/nginx/sites-available/projectLEMP`

Once the Nano editor opens, paste the below command and proceed to save


```javascript
#/etc/nginx/sites-available/projectLEMP

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
```


![Alt text](<Images/Open a new configuration file in Nginx in projectLEMP 1.PNG>)


![Alt text](<Images/Open a new configuration file in Nginx in projectLEMP 2.PNG>)


![Alt text](<Images/Open a new configuration file in Nginx in projectLEMP 3.PNG>)


Activate the configuration by linking to the config file from Nginx *site-enabled* directory.

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

![Alt text](<Images/Activate the configuration by linking to the config file from Nginx site-enabled directory.PNG>)

The below command will tell Nginx to use the configuration next time it is reloaded. To test the config syntax error by typing the below command.

`sudo nginx -t`

![Alt text](<Images/Test Nginx configuration syntax error.PNG>)

We need to now disable the default Nginx host that is currently configured to listen on port 80, please run the command below for this to be done.

`sudo unlink /etc/nginx/sites-enabled/default`

![Alt text](<Images/Disable the default Nginx host that is currently configured to listen on port 80.PNG>)

Reloead Nginx to apply the changes

`sudo systemctl reload nginx`

![Alt text](<Images/Reload Nginx to apply the changes.PNG>)

The new website is now active, however, the web root */var/www/projectLEMP* is still empty. Create an *index.html* file inside the projectlamp directory so we can test that the virtual host works as expected.

![Alt text](<Images/Create and index.html inside the projectLEMEP folder.PNG>)

After creating the *index.html* file, copy the below command and paste in your script shell local host

`sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

![Alt text](<Images/Curl the virtual host using the echo command.PNG>)

To test my local host access using the shell script

`curl http://127.0.0.1:80`

![Alt text](<Images/Test my local host access using the Shell..PNG>)

To try to open my website through my web browser, using the my public IP, run the below command.

`http://<Public-IP-Address>:80`

![Alt text](<Images/Open my website through my web browser, using the my public IP.PNG>)


# Testing PHP with Nginx

Inside the ProjectLEMP folder, create a LEMP file with the name *info.php*.

`touch info.php`

![Alt text](<Images/Create an info.php files.PNG>)

Create a file editor using the below command

`vi /var/www/projectLEMP/info.php`

Once the eidtor opens, paste the below command and save.

```javascript
<?php
phpinfo();
```
![Alt text](<Images/Paste the PHP value inside the file editor 1.PNG>)

![Alt text](<Images/Paste the PHP value inside the file editor.PNG>)

Now, we can access this page in the web browser by visiting the domain name or public IP address we had setup in the Nginx configuration file.

```javascript
http://`server_domain_or_IP`/info.php
```

![Alt text](<Images/Access the PHP configuration page content in the web browser.PNG>)

Due to the sensitivity of the information contained in my PHP Server and Ubuntu environment, I will proceed to delete the *info.php* file immediately using the below command

`sudo rm /var/www/your_domain/info.php`

![Alt text](<Images/Delete info.php due to content sensitivity.PNG>)

# Retrieving Data from SQL Database with PHP 

##### We will be creating a sample database called *example_database* and a username called *example_user*. We will create this user with the *mysql_native_password* authentication method in order to be able to connect to the MySQL database from PHP. Also, at this time, the native MySQL PHP library *mysqlnd* doesn't support *calling_sha2_authentication*, the default authentication method for MySQL8.

### Connect to MySQL using the below command, provide your MySQL password to gain entrance to the database

`sudo mysql -p`

##### Create a new Database by running the below command

```javascript
CREATE DATABASE example_database;
```

![Alt text](<Images/Create & Show Database.PNG>)

#### Create a new user and grant him full priviledges on the database you have just created. create a new user with the name *example_user*, using mysql_native_password as default authentication method. We're defining the user's password as *PassWord.1* but will be replacing the password with a more secured password.  

```javascript
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

![Alt text](<Images/Create User in MySQL.PNG>)

We need to give permission to the user over the *example_database* database. 

```javascript
GRANT ALL ON example_database.* TO 'example_user'@'%';
```

![Alt text](<Images/Grant Database permission to created user.PNG>)

Now, exit from MySQL

`exit`

### Login using the users credentials to confirm if he has the appropiate permission

`mysql -u example_user -p`

![Alt text](<Images/Logged in using the newly created user.PNG>)

Show the list of MySQL Databases

`SHOW DATABASES;`

![Alt text](<Images/Show MySQL Databases.PNG>)

We will create a table called todo_list using the command below 

`CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));`

![Alt text](<Images/Create a table called todo_list.PNG>)

Insert few rows of contents inside the test table using the command below

`INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

![Alt text](<Images/Insert Rows of contents.PNG>)

To confirm that the data was successfully saved in your to the table, run the below command

`SELECT * FROM example_database.todo_list;`

![Alt text](<Images/Confirm the data was saved to your table.PNG>)

Once this is confirmed, exit from the database

`exit`

Now, I will create a PHP script that will connect to MySQL and query for the content. Create a new PHP file called *todo_list.php* from the custom root directory using the below command

`nano /var/www/projectLEMP/todo_list.php`

<Images/Create a new PHP file called todo_list.php>

Copy the below content into the nano file editor. Kindly note that the below PHP script connects to the MySQL database and queries for the content of the todo_list table; displays the result in a list. 

```javascript
<?php
$user = "example_user";
$password = "PassWord.1";
$database = "example_database";
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
```

![Alt text](<Images/Script connects to MySQL and queries the content of the todo_list table.PNG>)

#### I can now access the page in my web broswer by visiting the public IP address for my website follwoed by */todo_list.php:*.

`http://<Public_domain_or_IP>/todo_list.php`

![Alt text](<Images/Access the page through the web browser.PNG>)


# THANK YOU!!