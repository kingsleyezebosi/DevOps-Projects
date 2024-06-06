

# AUTOMATING LOAD-BALANCER CONFIGURATION WITH SHELL SCRIPTING

###### This course is an introduction to automation

###### This projects demonstrates how to automate the setup and maintenance of loadbalancer using a freestyle job, enhancing efficiency and and reducing manual effort. As a DevOps engineer, automation is the core of our responsibility. Automation helps us speed up deployment of services and reduces the chances of error in everyday activities in the CICD process.

### Automate the deployment of webservers

### Deploying and configuring 2 webservers

Step 1:

- Provision two EC2 Instances

Open Port 80 & 8000 repectively on all servers to allow traffic from AnyWhere

![alt text](<Images/Create EC2 Instance for Web Server 1.PNG>)

![alt text](<Images/Create EC2 Instance for Web Server 2.PNG>)

![alt text](<Images/Create EC2 Instance for LoadBal.PNG>)

Connect to the webserver & Load Balancer using SSH

![alt text](<Images/Connect the EC2 instance Sever 1.PNG>)

![alt text](<Images/Create EC2 Instance for Web Server 2.PNG>)

![alt text](<Images/Create EC2 Instance for LoadBal.PNG>)

Run the apt update command in the terminal for Server 1 & 2

```javascript
sudo apt update -y
```

![alt text](<Images/sudo apt update.PNG>)

Open a file, paste the script below in the 2 web servers and close the file using the command below;

```javascript
sudo nano install.sh
```

```javascript
#!/bin/bash

####################################################################################################################
##### This automates the installation and configuring of apache webserver to listen on port 8000
##### Usage: Call the script and pass in the Public_IP of your EC2 instance as the first argument as shown below:
######## ./install_configure_apache.sh 127.0.0.1
####################################################################################################################

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure

PUBLIC_IP=$1

[ -z "${PUBLIC_IP}" ] && echo "Please pass the public IP of your EC2 instance as an argument to the script" && exit 1

sudo apt update -y &&  sudo apt install apache2 -y

sudo systemctl status apache2

if [[ $? -eq 0 ]]; then
    sudo chmod 777 /etc/apache2/ports.conf
    echo "Listen 8000" >> /etc/apache2/ports.conf
    sudo chmod 777 -R /etc/apache2/

    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8000>/' /etc/apache2/sites-available/000-default.conf

fi
sudo chmod 777 -R /var/www/
echo "<!DOCTYPE html>
        <html>
        <head>
            <title>My EC2 Instance</title>
        </head>
        <body>
            <h1>Welcome to my EC2 instance</h1>
            <p>Public IP: "${PUBLIC_IP}"</p>
        </body>
        </html>" > /var/www/html/index.html

sudo systemctl restart apache2

```

![alt text](<Images/Script pasted server 1.PNG>)

![alt text](<Images/Script pasted server 2.PNG>)

Step 5: Change the permissions for both servers on the file to make it executable using the command below

```javascript
sudo chmod +x install.sh
```

![alt text](<Images/chmod permission on Servers.PNG>)

Step 6: Run the shell script using the command below. Read the instructions in the shell script to learn how to use it.
    - Public IP of my 2 web server created in the EC2 instance - Web Server 1 - 13.51.198.3 & Web Server 2 - 13.53.197.31

```javascript
./install.sh PUBLIC_IP
```

![alt text](<Images/install public IP of my current EC2 Instance - Web Server 1.PNG>)

![alt text](<Images/install public IP of my current EC2 Instance - Web Server 2.PNG>)

Web Browser Display for the Web Server

![alt text](<Images/Web Browser - Server 1.PNG>)

# Deployment of Nginx as a Load blalancer using shell script

## Deploying and Configuring Nginx Load Balancer

Having successfully deployed and configured the two webservers. I provisioned another EC2 instance running ubuntu 22. I opened port 80 to anywhere using the security group and connected the loadbalancer via the terminal.

Created another EC2 Instance as a load balancer and with port 80 opened

![alt text](<Images/LoadBal Port 80.PNG>)

#### Setup and run shell script

Open a file nginx.sh using the command sudo nano nginx.sh in the load balancer

```javescript
sudo nano nginx.sh
```

I copied and pasted the script below:


```javascript
#!/bin/bash

######################################################################################################################
##### This automates the configuration of Nginx to act as a load balancer
##### Usage: The script is called with 3 command line arguments. The public IP of the EC2 instance where Nginx is installed
##### the webserver urls for which the load balancer distributes traffic. An example of how to call the script is shown below:
##### ./configure_nginx_loadbalancer.sh PUBLIC_IP Webserver-1 Webserver-2
#####  ./configure_nginx_loadbalancer.sh 127.0.0.1 192.2.4.6:8000  192.32.5.8:8000
############################################################################################################# 

PUBLIC_IP=$1
firstWebserver=$2
secondWebserver=$3

[ -z "${PUBLIC_IP}" ] && echo "Please pass the Public IP of your EC2 instance as the argument to the script" && exit 1

[ -z "${firstWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the second argument to the script" && exit 1

[ -z "${secondWebserver}" ] && echo "Please pass the Public IP together with its port number in this format: 127.0.0.1:8000 as the third argument to the script" && exit 1

set -x # debug mode
set -e # exit the script if there is an error
set -o pipefail # exit the script when there is a pipe failure


sudo apt update -y && sudo apt install nginx -y
sudo systemctl status nginx

if [[ $? -eq 0 ]]; then
    sudo touch /etc/nginx/conf.d/loadbalancer.conf

    sudo chmod 777 /etc/nginx/conf.d/loadbalancer.conf
    sudo chmod 777 -R /etc/nginx/

    
    echo " upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server  "${firstWebserver}"; # public IP and port for webserser 1
            server "${secondWebserver}"; # public IP and port for webserver 2

            }

           server {
            listen 80;
            server_name "${PUBLIC_IP}";

            location / {
                proxy_pass http://backend_servers;   
            }
    } " > /etc/nginx/conf.d/loadbalancer.conf
fi

sudo nginx -t

sudo systemctl restart nginx

```

![alt text](<Images/Script pasted in load balancer.PNG>)

Step 3: Run the chmod permission command below;

```javascript
sudo chmod +x nginx.sh
```

![alt text](<Images/chmod permission on Loadbal.PNG>)

Step 4: Run the script with the command below;

    Note: Public IP of the load balancer which is the EC2 instance and the public IPs of the 2 web servers I'd created for this project 8

```javascript
./nginx.sh 16.171.255.181 13.51.198.3:8000 13.53.197.31:8000
```

![alt text](<Images/Run the script to then verify the webserver automation.PNG>)

Load Balance Server ported to the first Server

![alt text](<Images/Web Browser - LoadBal.PNG>)


Load Balance Server ported to the second Server

![alt text](<Images/Web Browser - LoadBal 1.PNG>)




# THANK YOU!!!
