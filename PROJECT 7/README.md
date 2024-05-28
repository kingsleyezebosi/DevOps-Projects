

# IMPLEMENTING LOAD BALANCING WITH NGINX 

## Introduction to Load Balancing and Nginx


![alt text](<Images/load balancing.png>)

### What is a Load Balancer

A load balancer distributes workloads across multiple compute resources, such as virtual servers. Using a load balancer increases the availability and fault tolerance of your applications.

You can add and remove compute resources from your load balancer as your needs change, without disrupting the overall flow of requests to your applications.

The load balancer stands in front of the webserver and all traffic gets into it first, it then distribute the traffic evenly across the set of webservers. This ensures no webserver gets overworked,consequently improving performance.

Nginx is a versatile software it acts like a webserver,reverse proxy, and a load balancer etc.All that is needed is to configure it properly to server to your use case.

## Setting Up a BAsic Load Balancer

Step 1: Provision EC2 Instance

Open the AWS Console and create the instances

![alt text](<Images/Created 2 EC2 Instances.PNG>)

Step 2: Open Port 8000, we will be running our webservers on port 8000 while the load balancers run port 80. we will need to open port 8000 to allow traffic from anywhere. To do this we need to add a rule to the security group of each of our webserver.

Click On the instance ID to get of your EC2 instances.

- Load-balancer - Apache

![alt text](<Images/Load Balancer Apache 1.PNG>)

- Load Balancer - Nginx

![alt text](<Images/Load Balancer Nginx 1.PNG>)

- Scroll down to Security on both servers, edit the inbound rules for both servers and set it to 8000, ipv4Anywhere

- Enabled on Apache

![alt text](<Images/Rule enabled on Apache.PNG>)

- Enabled on Nginx Server

![alt text](<Images/Rule enabled on Nginx.PNG>)

- Connect via ssh for both Servers

- Apache 1

![alt text](<Images/Connect via ssh for Apache 1.PNG>)

- Apache 2

![alt text](<Images/Connect via ssh for Apache 2.PNG>)

Step 3: Install Apache on both Webservers using the below commands;

```javascript
sudo apt update -y
```

Webserver 1

![alt text](<Images/sudo apt update on Apache 1.PNG>)

Webserver 2

![alt text](<Images/sudo apt update apache 2.PNG>)

```javascript
sudo apt install apache2 -y
```

Webserver 1

![alt text](<Images/sudo apt install Apache 1.PNG>)

Webserver 2

![alt text](<Images/Install apache 2.PNG>)


```javascript
sudo systemctl status apache2
```

![alt text](<Images/confirm apache install status.PNG>)

![alt text](<Images/systemctl status apache 2.PNG>)

Step 4: Configuring Apache webservers to serve content on port 8000 instead of the default port which is port 80. Then we will create new index.html file. The file contain code to display the public Ip address of the Ec2 instances . We will then override apache webserver default html file with the new file

- To configure Apache to serve content on port 8000.

- Using the nano text editor, open the file/etc/apache2/ports.conf

```javascript
sudo nano /etc/apache2/ports.conf 
```

Apache 1

![alt text](<Images/Listen 8000 Apache 1.PNG>)

Apache 2

![alt text](<Images/Listen 8000 Apache 2.PNG>)

- On both servers, open the file /etc/apache2/sites-available/000-default.conf , and change port 80 on the virtualhost to 8000.

```javascript
sudo nano /etc/apache2/sites-available/000-default.conf
```

Server 1

![alt text](<Images/change virtual host port apache 1.PNG>)

Server 2

![alt text](<Images/change virtual host port apache 2.PNG>)


Restart Apache to load the new configuration for both server

```javascript
sudo systemctl restart apache2
```

![alt text](<Images/restart the configuration.PNG>)

On both servers, create new index.html file and paste code

```javascript
sudo nano index.html
```

Server 1

![alt text](<Images/new html index server 1.PNG>)

Server 2

![alt text](<Images/new html index server 2.PNG>)

Change File ownership of index.html file

```javascript
sudo chown www-data:www-data ./index.html
```

Overide the default html file of the 2 Apache webservers

```javascript
sudo cp -f ./index.html /var/www/html/index.html
```

Restart webserver to load new configurations

```javascript
sudo systemctl restart apache2
```

Server 1

![alt text](<Images/chown permission - server 1.PNG>)

Server 2

![alt text](<Images/chown permission - server 2.PNG>)


A Welcome Web Browser Apache 1 & 2 Servers

![alt text](<Images/Welcome Web Page Apache 1.PNG>)

![alt text](<Images/Welcome Web Page Apache 2.PNG>)


Step 5: Configure Nginx as a Load Balancer

Configuring Nginx as a Load Balancer

Provision a new EC2 instance

![alt text](<Images/EC2 Nginx for Load Balancing.PNG>)

Open Port 80 for Traffic AnyWhere

![alt text](<Images/Open port 80 for Nginx.PNG>)

SSH into instance

Install Nginx

```javascript
sudo apt update -y && sudo apt install nginx -y
```

![alt text](<Images/Nginx sudo apt update.PNG>)

![alt text](<Images/Nginx sudo apt install.PNG>)

Verify Nginx is installed and working

```javascript
sudo systemctl status nginx
```

![alt text](<Images/Nginx systemctl restart.PNG>)

Open Nginx Configuration File

```javascript
sudo nano /etc/nginx/conf.d/loadbalancer.conf
```

Paste the below configuration file where you are going to paste the code for nginx to act as a load-balancer



```javascript
        
        upstream backend_servers {

            # your are to replace the public IP and Port to that of your webservers
            server 16.16.203.108:8000; # public IP and port for webserser 1
            server 16.170.165.33:8000; # public IP and port for webserver 2

        }

        server {
            listen 80;
            server_name 16.16.167.103>; # provide your load balancers public IP address

            location / {
                proxy_pass http://backend_servers;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            }
        }
    
```

![alt text](<Images/Nginx edited configuration file.PNG>)

IMPORTANT NOTES:

-Upstream backend_servers : Define a group of backend servers
-The server lines inside the upstream block list the addresses and the ports of your backend-servers.
-Proxy_pass inside the location block sets up the load balancing, passing the request to the backend servers.
-The proxy_set_header lines pass necessary header to the backend servers to correctly handled the request.

###### TESTING CONFIGURATION

To test Configuration, Run 

```javascript
sudo nginx -t
```

![alt text](<Images/Ngix texting configuration file.PNG>)

Restart Nginx to load the configuration

```javascript
sudo systemctl restart nginx
```

![alt text](<Images/Nginx systemctl restart again.PNG>)


- In the web browser paste the Ip-address of the Nginx load paste and hit enter.

- THE RESULT

    - Load-balancer apache

![alt text](<Images/Nginx Load Balanced 1.PNG>)

![alt text](<Images/Nginx Load Balanced 2.PNG>)


THANK YOU!!

















































