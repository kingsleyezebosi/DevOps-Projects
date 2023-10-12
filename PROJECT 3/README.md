# LAMPSTACK IMPLEMENTATION PROJECT
### What is Lampstack?

A LAMP stack is a bundle of four different software technologies that developers use to build websites and web applications. LAMP is an acronym for the operating system, Linux; the web server, Apache; the database server, MySQL; and the programming language, PHP. All four of these technologies are open source, which means they are community maintained and freely available for anyone to use. Developers use LAMP stacks to create, host, and maintain web content. It is a popular solution that powers many of the websites you commonly use today.

# What is a Stack?
A technological stack is a set of frameworks, libraries and tools used to develop a Software Application. These include : LAMP STACK, LEMP STACK, MEAN STACK AND MERN STACK.

# PREREQUISITE (LAMP STACK IMPLEMENTATION)
- An Aws Free tier account.

- An Ec2 instance Running on a virtual machine

- Ubuntu server Os running.

# AWS ACCOUNT SETUP
- Registered and setup an AWS account 

![Alt text](<images/EC2 Instance key pair.PNG>)

- Create an access key pair and this will be downloaded automatically.

![Alt text](<images/EC2 Instance key pair.PNG>)

### Connecting to Aws Apache Instance using a terminal called Git Bash
Choose SSH client and copy SSH address

![Alt text](<images/connect to instance using the ssh client.PNG>)

Connected to EC2 instance using terminal using ssh in Git Bash

![Alt text](<images/connected to EC2 instance using ssh in Git Bash 1.PNG>)



# Installing Apache and Updating the Firewall

What is Apache?

Apache is an open-source web server that forms the second layer of the LAMP stack. The Apache module stores website files and exchanges information with a browser using HTTP, an internet protocol for transferring website information in plain text. For example, when a browser requests a webpage, the Apache HTTP server does the following: Receives the request, Processes the request and finds the required page file and Sends the relevant information back to the browser

Update the list of packages in package manager using the below command.

`sudo apt update`

![Alt text](<images/sudo apt update.PNG>)


Run apache2 package installation using the below command.

`sudo apt install apache2`

![Alt text](<images/sudo apt install apache2.PNG>)

To verify apache2 is running as a service in the OS

`sudo systemctl status apache2`

![Alt text](<images/sudo systemctl status apache2.PNG>)

Before we can receive any traffic by our webserver, we need to open TCP port 80, which is the default port that our web browsers use to access web pages on the internet.

We have the TCP port 22 that opens by default on our EC2 machine to access it via the SSH, however, we need to add a rule to EC2 configuration to open inbound connection through port 80.

Open TCP port 80, which is the default web port for web browswers to access a web page

![Alt text](<images/HTTP Port 80 Inbound Rule.PNG>)

- To access Apache server via local machine

Run curl http://localhost:80

or curl http://127.0.0.1:80

![Alt text](<images/curl localhost port 80.PNG>)

To test how Apache server responds to HTTP request from the internet.

`http://44.202.138.37:80`

![Alt text](<images/Apache Server response on web.PNG>)

To retrieve your AWS EC2 Instance Public IP address through the terminal, please run the below command.

`curl -s http://169.254.169.254/latest/meta-data/public-ipv4`

![Alt text](<images/To retrieve your AWS public IP address throught the terminal.PNG>)


# Installing Mysql
### MySQL

MySQL is an open-source relational database management system and is the third layer of the LAMP stack. The LAMP model uses MySQL for storing, managing, and querying information in relational databases. For example, developers store application data, such as customer records, sales, and inventories. When a user searches for information, the web server queries the stored data in MySQL. Query refers to special instructions for manipulating data in a relational database with the SQL language.

`sudo apt install mysql-server`

![Alt text](<images/sudo apt install mysql-server.PNG>)

`sudo mysql`

![Alt text](<images/sudo mysql.PNG>)

To set the password for the root user, using mysql_native_password as default authentication method. We would define the user's password as *PassWord.1*

- To set Password for the Root user
`ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

![Alt text](<images/Reset Password for Mysql.PNG>)

To Exit Mysql shell, run the below command

`exit` 
![Alt text](<images/Exit Mysql.PNG>)


# Start Mysql interactive script by running the command below.

`sudo mysql_secure_installation`

![Alt text](<images/sudo mysql secure installation.PNG>)

- To Confirm password change for Mysql root user and change anonymous user

- Click y/ any other letter

![Alt text](<images/Mysql Password Change.PNG>)

![Alt text](<images/Mysql complete installation.PNG>)

To Login to Mysql console

`sudo mysql -p`

![Alt text](<images/Login to Mysql console.PNG>)

To exit Mysql console

`exit`

![Alt text](<images/To exit from Mysql Console.PNG>)

# Step 3: Installing PHP

##### The word PHP means Hypertext Preprocessor, is the fourth and final layer of the LAMP stack. It is a scripting language that allows websites to run dynamic processes. A dynamic process involves information in software that constantly changes. Web developers embed the PHP programming language in HTML to show real-time or updated information on websites. They use PHP to allow the web server, database, and operating system to cohesively process requests from browsers. 

Installation Prerequsites

1. Install PHP-MYSQL - This would enable PHP communicate with Mysql
2. Install libapache2-mod-php to enable apache haandle PHP files
3. Intall PHP packages which would install its dependecies

To install the php-mysql, libapache2-mod-php and php package

`sudo apt install php libapache2-mod-php php-mysql`

![Alt text](<images/Installing PHP.PNG>)

![Alt text](<images/PHP Installation with Pending kernel upgrade.PNG>)

To confirm the PHP version

`php -v`

![Alt text](<images/check php version.PNG>)

# Enable PHP on the Website

`sudo vim /etc/apache2/mods-enabled/dir.conf`

### Paste the below command inside the nano pop-up

```javascript
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```

![Alt text](<images/Enable PHP on a website 1 paste in nano pop up.PNG>)

![Alt text](<images/Enable PHP on a website.PNG>)

Reload Apache using the below command.

`$ sudo systemctl reload apache2`

![Alt text](<images/Reload the apache2 server.PNG>)

#### Create a new file named *index.php* inside the custom root folder

To cd into the root directory

`sudo -i`
![Alt text](<images/Login to Root Directory.PNG>)

cd inside the root directory absolute path 

`cd /var/www/`

![Alt text](<images/Root Directory Absolute paths var and www.PNG>)

Create a folder called *projectlamp*

`sudo mkdir /var/www/projectlamp`

![Alt text](<images/Create a projectlamp folder in the root dir.PNG>)

Assign ownership of the directory to the $USER environment variable, which will reference the current system user

`sudo chown -R $USER:$USER /var/www/projectlamp`

![Alt text](<images/Assign ownership of the root directory.PNG>)

Then, create and open a new configuration file in Apache's *site-available* directory using the *vi* command.

`sudo vi /etc/apache2/sites-available/projectlamp.conf`

![Alt text](<images/Create and open a new configuration file in Apache's site-available directory using the vi command.PNG>)

You can use the *ls* command to show the new file in the *site-available* directory

`sudo ls /etc/apache2/sites-available`

*You will see something like this as show in the below parameter*
000-default.conf  default-ssl.conf  projectlamp.conf

![Alt text](<images/Use the ls command to show the new file in the site-available directory.PNG>)

To Enable Virtual Host, please run the below command.

`sudo a2ensite projectlamp`

![Alt text](<images/To enable virtual host on projectlamp.PNG>)

We would need to disable the default website that comes installed with Apache. This is required if you are not using a custom domain name, because in this case Apache's default configuration would overwrite the virtual host. To disable to default Apache's website, please run the below command

`sudo a2dissite 000-default`

![Alt text](<images/To disable Apache's default website.PNG>)

To ensure that the configuration file does not contain syntax errors, we run the below command

![Alt text](<images/To ensure that the configuration file does not contain syntax errors.PNG>)

Reload Apache so that these changes will take effect.

`sudo systemctl reload apache2`

![Alt text](<images/Reload apache to effect all changes.PNG>)

The new website is now active, however, the web root */var/www/projectlampt* is still empty. Create an *index.html* file inside the projectlamp directory so we can test that the virtual host works as expected.

![Alt text](<images/Create an index.html file inside the projectlamp folder.PNG>)

After creating the *index.html* file, copy the below command and paste in your script shell local host

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`

![Alt text](<images/Curl the virtual host using the echo command.PNG>)

To test my local host access using the shell script

`curl http://127.0.0.1:80`

![Alt text](<images/Test my local host access using the Shell.PNG>)

To try to open my website through my web browser, using the my public IP, run the below command.

`http://<Public-IP-Address>:80`

![Alt text](<images/Open my website through the web browser using my Public IP.PNG>)


# THANK YOU!!