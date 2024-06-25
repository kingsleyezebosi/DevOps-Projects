

# DevOps-Tooling-Website-Solution - Kingsley

### Introduction

In this Project I will implement a set of DevOps tools that will help a DevOps team in day to day activities in managing, developing, testing, deploying and monitoring different projects.

These tools are well known and widely used by multiple DevOps teams. This single DevOps Tooling Solution will consist of:

1 Jenkins - free and open source automation server used to build CI/CD pipelines.

2 Kubernetes - an open-source container-orchestration system for automating computer application deployment, scaling, and management.

3 Jfrog Artifactory - Universal Repository Manager supporting all major packaging formats, build tools and Cl servers. Artifactory.

4 Rancher - an open source software platform that enables organizations to run and manage Docker and Kubernetes in production.

5 Grafana - a multi-platform open source analytics and interactive visualization web application.

6 Prometheus - An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.

7 Kibana - Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.


#### Side Self Study

#### Network-attached storage (NAS)

Network-attached storage (NAS) is a file-level (as opposed to block-level storage) computer data storage server connected to a computer network providing data access to a heterogeneous group of clients.

The term "NAS" can refer to both the technology and systems involved, or a specialized device built for such functionality (as unlike tangentially related technologies such as local area networks, a NAS device is often a singular unit).

### Storage Area Network (SAN)

A storage area network (SAN) or storage network is a computer network which provides access to consolidated, block-level data storage.

SANs are primarily used to access data storage devices, such as disk arrays and tape libraries from servers so that the devices appear to the operating system as direct-attached storage.

A SAN typically is a dedicated network of storage devices not accessible through the local area network (LAN).

### NFS

Network File System (NFS) is a distributed file system protocol originally developed by Sun Microsystems (Sun) in 1984, allowing a user on a client computer to access files over a computer network much like local storage is accessed. NFS, like many other protocols, builds on the Open Network Computing Remote Procedure Call (ONC RPC) system. NFS is an open IETF standard defined in a Request for Comments (RFC), allowing anyone to implement the protocol.

### FTP

The File Transfer Protocol (FTP) is a standard communication protocol used for the transfer of computer files from a server to a client on a computer network.

FTP is built on a client–server model architecture using separate control and data connections between the client and the server.

FTP users may authenticate themselves with a plain-text sign-in protocol, normally in the form of a username and password, but can connect anonymously if the server is configured to allow it.

For secure transmission that protects the username and password, and encrypts the content, FTP is often secured with SSL/TLS (FTPS) or replaced with SSH File Transfer Protocol (SFTP).

### SMB

Server Message Block (SMB) is a communication protocol mainly used by Microsoft Windows equipped computers normally used to share files, printers, serial ports, and miscellaneous communications between nodes on a network.

SMB implementation consists of two vaguely named Windows services: "Server" (ID: LanmanServer) and "Workstation" (ID: LanmanWorkstation).

It uses NTLM or Kerberos protocols for user authentication. It also provides an authenticated inter-process communication (IPC) mechanism.

### iSCSI

Internet Small Computer Systems Interface or iSCSI is an Internet Protocol-based storage networking standard for linking data storage facilities.

iSCSI provides block-level access to storage devices by carrying SCSI commands over a TCP/IP network. iSCSI facilitates data transfers over intranets and to manage storage over long distances.

It can be used to transmit data over local area networks (LANs), wide area networks (WANs), or the Internet and can enable location-independent data storage and retrieval.

### Block-level storage is and how it is used by Cloud Service providers

Block-level storage is a concept in cloud-hosted data persistence where cloud services emulate the behaviour of a traditional block device, such as a physical hard drive.

Developers use block storage to store containerized applications on the cloud. Containers are software packages that contain the application and its resource files for deployment in any computing environment. Like containers, block storage is equally flexible, scalable, and efficient.

Storage in such services is organised as blocks. This emulates the type of behaviour seen in traditional disks or tape storage through storage virtualization.

Blocks are identified by an arbitrary and assigned identifier by which they may be stored and retrieved, but this has no obvious meaning in terms of files or documents.

A file system must be applied on top of the block-level storage to map 'files' onto a sequence of blocks. Amazon EBS (elastic block store) is an example of a cloud block store.

### Difference between Block-level storage and Object storage

Object storage normally uses a distributed storage environment across multiple different storage nodes or servers.

On the other hand, block storage uses RAID, SSDs, and hard disk drives (HDDs) for storage.

Finally, cloud file storage uses network-attached storage (NAS) in an on-premises setup.

### AWS services

Amazon Web Services (AWS) is the world's most comprehensive and broadly adopted cloud, offering over 200 fully featured services from data centers globally.

### Difference between Block Storage, Object Storage and Network File System.

Object storage normally uses a distributed storage environment across multiple different storage nodes or servers.

On the other hand, block storage uses RAID, SSDs, and hard disk drives (HDDs) for storage.

Finally, cloud file storage uses network-attached storage (NAS) in an on-premises setup.

### Project Objective

    In this project I will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.

In this project I will implement a solution that consists of following components:

1 Infrastructure: AWS

2 Webserver Linux: Red Hat Enterprise Linux 8

3 Database Server: Ubuntu 20.04+ MySQL

4 Storage Server: Red Hat Enterprise Linux 8 + NFS Server

5 Programming Language: PHP

6 Code Repository: GitHub


    The diagram below shows a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage.

![alt text](<Images/3 tier web application.jpg>)


    Even though the NFS server might be located on a completely separate hardware.

    For Web Servers it look like a local file system from where they can serve the same files.

It is important to know what storage solution is suitable for what use cases, for this - you need to answer following questions:

    what data will be stored

    in what format

    how this data will be accessed

    by whom, from where

    how frequently

    etc.

    Base on this you will be able to choose the right storage system for your solution.

For Rhel 8 server use this ami

```javascript
RHEL-8.6.0_HVM-20220503-x86_64-2-Hourly2-GP2 (ami-035c5dc086849b5de)
```

## Implementing a business website using NFS for the backend file storage

### Step 1 - Prepare NFS Server

Spin up a new EC2 instance with RHEL Linux 8 Operating System.

![alt text](<Images/EC2 Instance for NFS.PNG>)

To install the lvm2 package run the below command;

```javascript
 sudo yum install lvm2
```

![alt text](<Images/lvm2 package installed.PNG>)

![alt text](<Images/Create 3 volumes.PNG>)


- To see all mount and free space on the web-server

```javascript
df -h
```

Create single partitions

- To create a single partition on each of the disk using the gdisk Utility, run the below command:

```javascript
sudo gdisk /dev/nvme1n1
```

```javascript
sudo gdisk /dev/nvme2n1
```

```javascript
sudo gdisk /dev/nvme3n1
```

![alt text](<Images/Create partitions.PNG>)

Run the below command to check for available partitions

```javascript
sudo lvmdiskscan
```

- To mark each of the three disks as physical Volumes(Pvs) to be used by LVM, run the below commands

```javascript
sudo pvcreate /dev/nvme1n1p1 
```

```javascript
sudo pvcreate /dev/nvme2n1p1 
```

```javascript
sudo pvcreate /dev/nvme3n1p1
```

- To verify that the physical volume has been created successfully

```javascript
sudo pvs
```

![alt text](<Images/Add all physical volumes.PNG>)

- To add all the three Physical volumes(Pvs) to a Volume Group (VG). Name the VG webdata-vg

```javascript
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```

![alt text](<Images/sudo vgcreate.PNG>)

To create 3-logic Volumes lv-opt, lv-apps and lv-logs, run the below commands;

```javascript
sudo lvcreate -n lv-apps -L 9G webdata-vg 
```

```javascript
sudo lvcreate -n lv-logs -L 9G webdata-vg 
```

```javascript
sudo lvcreate -n lv-opt -L 9G webdata-vg
```

![alt text](<Images/Create the 3 logical vols - apps logs opt.PNG>)


To format the logical Volumes with xfs file system using mkfs.xfs

```javascript
sudo mkfs -t xfs /dev/webdata-vg/lv-opt
```

```javascript
sudo mkfs -t xfs /dev/webdata-vg/lv-apps
```

```javascript
sudo mkfs -t xfs /dev/webdata-vg/lv-logs
```

![alt text](<Images/Format the 3 logical vols.PNG>)

Create mount points on /mnt directory for the logical volumes as follow:

```javascript
sudo mkdir /mnt/apps
```

```javascript
sudo mkdir /mnt/logs
```

```javascript
sudo mkdir /mnt/opt
```

Note that the 3 above mount points signifies the following;

    - Mount lv-apps on /mnt/apps - To be used by webservers

    - Mount lv-logs on /mnt/logs - To be used by webserver logs

    - Mount Iv-opt on /mnt/opt - To be used by Jenkins server in Project 8


![alt text](<Images/Create 3 dir mount points.PNG>)


To mount /mnt/apps on lv-apps logical volume

```javascript
sudo mount /dev/webdata-vg/lv-apps /mnt/apps
```

    - To mount /mnt/logs on lv-apps

```javascript
sudo mount /dev/webdata-vg/lv-logs /mnt/logs
```

    - To mount /mnt/logs on lv-logs

```javascript
sudo mount /dev/webdata-vg/lv-opt /mnt/opt
```

    - To mount /mnt/logs on lv-opt


![alt text](<Images/Mount mnt-apps on lv apps logic vol.PNG>)


Use rsync utility to backup all the files in the log directory /var/log into /mnt/logs (This is required before mounting the file system).

```javascript
sudo rsync -av /var/log/. /mnt/logs
```

![alt text](<Images/Use resync utilities.PNG>)

To Mount /var/log on logs-lv logical volume. (Note that all the existing data on /var/log will be deleted).

```javascript
sudo mount /dev/webdata-vg/lv-logs /var/log
```

![alt text](<Images/mount vg lv  to var log.PNG>)

To Restore log files back into /var/log directory.

```javascript
sudo rsync -av /mnt/logs/ /var/logs
```

![alt text](<Images/Restore log files to var log.PNG>)

To Update /etc/fstab file so that the mount configuration will persist after restarting the server. The UUID of the device will be used to update the /etc/fstab file. Run the command shown below to get the UUID of the lv-apps, lv-logs and lv-opt logical volumes: Run the below command

```javascript
sudo blkid
 ```

![alt text](<Images/copy UUID of apps logs & opts.PNG>)

apps: UUID=e8a72e64-1673-47d2-8f56-43879e6e85cb /mnt/apps xfs defaults 0 0

opt: UUID=06c19604-1e69-4de3-a76c-5fd8d1292230 /mnt/opt xfs defaults 0 0

logs: UUID=60e4df1e-c98c-472b-ba58-5d0fa8c2ee9e /mnt/logs xfs defaults 0 0

Update /etc/fstab in this format using your own UUID and remember to remove the leading and ending quotes.

```javascript
sudo vi /etc/fstab
```

![alt text](<Images/Paste the UUIDs inside the vi file.PNG>)

To Test the configuration, Run

```javascript
sudo mount -a
```

To Reload the daemon , Run

```javascript
sudo systemctl daemon-reload

To verify the set up

```javascript
sudo df -h 
```

The df -h command comes in after relaoding the Deamon using sudo mount -a and sudo systemctl daemon-reload.

![alt text](<Images/Mount,  Reload & Verify setup.PNG>)


## Installing NFS-Server

To install NFS server, run the below command;

```javascript
sudo yum -y update
```

![alt text](<Images/NFS Server update.PNG>)

```javascript
sudo yum install nfs-utils -y
```

![alt text](<Images/NFS Server Installation 1.PNG>)

```javascript
sudo systemctl start nfs-server.service
```

```javascript
sudo systemctl enable nfs-server.service
```

```javascript
sudo systemctl status nfs-server.service 
```

![alt text](<Images/Install NFS Server.PNG>)


- Export the mount for webserver subnet cidr to connect as clients. For simplicity, you will install the three webservers inside the subnet, but in production set up you would probably want to seperate each tier inside its own subnet for higher level of security.

- To check your subnet cidr- Open your EC2 details in AWS web console and locate Networking tab and open a subnet link.

![alt text](<Images/Subnet ID.PNG>)

To make sure we set up a permission that will allow our webservers read, write and execute files on NFS Server

```javascript
sudo chown -R nobody: /mnt/apps
```

```javascript
sudo chown -R nobody: /mnt/logs
```

```javascript
sudo chown -R nobody: /mnt/opt
```

```javascript
sudo chmod -R 777 /mnt/apps
```

```javascript
sudo chmod -R 777 /mnt/logs
```

```javascript
sudo chmod -R 777 /mnt/opt
```

```javascript
sudo systemctl restart nfs-server.service
```

![alt text](<Images/Setup Chmod Permissions.PNG>)

To configure access to NFS for clients within the same subnets, run the below command;

```javascript
sudo vi /etc/exports
```

Paste the below information

/mnt/apps <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/logs <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)
/mnt/opt <Subnet-CIDR>(rw,sync,no_all_squash,no_root_squash)

Modified Version

/mnt/apps 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt  172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)

Then, Esc + :wq!

![alt text](<Images/vi into the export file and paste subnet information.PNG>)

Next, to export, run the below command;

```javascript
sudo exportfs -arv
```

![alt text](<Images/View subnet exported result.PNG>)

To check which port is used by NFS server and open it using security groups, run the below command

```javascript
rpcinfo -p | grep nfs
```

![alt text](<Images/Check IP ports used by NFS server.PNG>)


Important note: In order for NFS server to be accessible from your client , you must also open following ports ;

- TCP 111

- UDP 111

- UDP 2049

![alt text](<Images/Open 3 TCP-UDP IP Ports.PNG>)

![alt text](<Images/Open 3 TCP-UDP IP Ports 1.PNG>)


# Configure backend database as part of 3 tier architecture

### Step 2 - Configure the database server

By now you should know how to install and configure a MySQL DBMS to work with remote Web Server

    1. Install MySQL server
    2. Create a database and name it tooling
    3. Create a database user and name it webaccess
    4. Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr


![alt text](<Images/Connect MySQL using SSH.PNG>)

```javascript
sudo apt update -y
```

![alt text](<Images/sudo apt update.PNG>)


```javascript
sudo apt install mysql-server -y
```

![alt text](<Images/sudo apt install mysql.PNG>)

```javascript
sudo mysql
```

Run the below command in the database environment. % means - any IP range under that 

```javascript
sudo mysql
CREATE DATABASE tooling;
CREATE USER `myuser`@`<NFS-Server-Subnet CIDR-IP-Address>` IDENTIFIED BY 'mypass';
GRANT ALL ON tooling.* TO 'myuser'@'<NFS-Server-Subnet CIDR-IP-Address>';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

```javascript
CREATE DATABASE tooling;
CREATE USER 'admin'@'%' identified by 'password1';
GRANT ALL on tooling.* to 'admin'@'%';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit;
```

```javascript
CREATE DATABASE tooling;
CREATE USER 'webaccess'@'172.31.16.0/20' identified by 'password1';
GRANT ALL on tooling.* to 'webaccess'@'172.31.16.0/20';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit;
```

Check list of created users in MySQL Database

```javascript
SELECT User, Host FROM mysql.user;
```

![alt text](<Images/create DB, web access and admin access on MySQL 1.PNG>)

![alt text](<Images/create DB, web access and admin access on MySQL 2.PNG>)

![alt text](<Images/create DB, web access and admin access on MySQL.PNG>)

Change the bind-address and mysqlx-bind-addresses to 0.0.0.0

```javascript
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

Default view 

![alt text](<Images/Change MySQL bind address.PNG>)

Modified View

![alt text](<Images/Change MySQL bind address 1.PNG>)

```javascript
sudo systemctl restart mysql
```

```javascript
sudo systemctl status mysql
```

![alt text](<Images/Restart MySQL & Check MySQL Status.PNG>)

## STEP-3

#### Preparing the Webservers

we need to make sure that our webservers can serve the same content from the shared storage solutions, in our case NFS server and MySql database. You already know that one DB can be accessed for reads and writes by multiple clients.For storing shared files that our webservers willuse- we utilize NFS and ount previously created logical Volume Lv-apps to the folder where Apache stores files to be served to the users (/var/www)

This approach will make our Webserver stateless, which means we will bw ablw to add new ones or remove them whenever we need, and the intergrity of the data(in the database and on NFS) will be preserved.

During the next steps, we will do the following;

    - To configure NFS client(this step must be done on all three servers)

    - Deploy a Tooling application to our web servers into a shared NFS Folder

    - To configure the web-server to work with a single MySQL database.

1. Launch a new EC2 instance with RHEL 8 Operating System

![alt text](<Images/NFS Client EC 2 Instance.PNG>)

![alt text](<Images/RHEL 8.1.0.PNG>)

![alt text](<Images/SSH NFS Client - 1.PNG>)

2. Install NFS Client

```javascript
sudo yum install nfs-utils nfs4-acl-tools -y
```

![alt text](<Images/Install NFS Client.PNG>)

3. Mount /var/www/ and target the NFS server's export for apps

```javascript
sudo mkdir /var/www
```

```javascript
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
```

```javascript
sudo mount -t nfs -o rw,nosuid 172.31.21.116:/mnt/apps /var/www
```

![alt text](<Images/sudo mount NFS Client.PNG>)

4. Verify that NFS was mounted successfully by running of df -h . Make sure that the changes will persist on Web Server after reboot:

```javascript
sudo vi /etc/fstab
```

add following line

```javascript
<NFS-Server-Private-IP-Address>:/mnt/apps /var/www nfs defaults 0 0
```

```javascript
172.31.21.116:/mnt/apps /var/www nfs defaults 0 0
```

![alt text](<Images/df -h.PNG>)

![alt text](<Images/VI into the NFS file.PNG>)

5. Install Remi's repository, Apache and PHP

```javascript
sudo yum install httpd -y
```


```javascript
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

```javascript
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

```javascript
sudo dnf module reset php
```

```javascript
sudo dnf module enable php:remi-7.4
```

```javascript
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```

```javascript
sudo systemctl start php-fpm
```

```javascript
sudo systemctl enable php-fpm
```

```javascript
sudo setsebool -P httpd_execmem 1
```

![alt text](<Images/Install remi repo, Apache & PHP.PNG>)


# Repeat steps 1-5 for another 2 Web Servers.

6. Verify that Apache files and directories are available on the Web Server in /var/www and also on the NFS server in /mnt/apps.

If you see the same files - it means NFS is mounted correctly. You can try to create a new file touch test.txt from one server and check if the same file is accessible from other Web Servers.

Created a file called touch.md in Web 1 by navigating to cd /var/www 

![alt text](<Images/Created a file in NFS Client 1.PNG>)

The same file could be seen in my NFS Server by navigating to cd /mnt/apps

![alt text](<Images/Same file seen in my NFS Server.PNG>)


7. Locate the log folder for Apache on the Web Server 1 and mount it to NFS server's export for logs.

Repeat step No4 to make sure the mount point will persist after reboot.

On the Web 1 Server, run the below command

```javascript
sudo mount -t nfs -o rw,nosuid 172.31.21.116:/mnt/logs /var/log/httpd
```

![alt text](<Images/Locate Log fold on Apache and mount it to NFS.PNG>)

Vi into the Web 1 server and update the file using the below parameters:

#### 172.31.21.116:/mnt/logs /var/log/httpd nfs defaults 0 0

```javascript
sudo vi /etc/fstab
```

![alt text](<Images/VI into the NFS file.PNG>)


8. Fork the tooling source code from Darey.io Github Account to your Github account.

```javascript
sudo yum install git
```

![alt text](<Images/Install git.PNG>)

```javascript
git clone https://github.com/darey-io/tooling.git
```

![alt text](<Images/Fork the tooling source code from dare.io.PNG>)

9. Deploy the tooling website's code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html

```javascript
cd tooling
```

```javascript
sudo cp -R html/. /var/www/html
```

```javascript
ls /var/www/html
```

```javascript
ls html/
```

![alt text](<Images/Deploy the tooling source code to web server.PNG>)


Note 1: Do not forget to open TCP port 80 on the Web Server.

![alt text](<Images/Open port 80 on the NFS Server.PNG>)

![alt text](<Images/Open port 80 on the NFS Server 1.PNG>)

Note 2: If you encounter 403 Error-check permissions to your /var/www/html folder and also disable SELinux sudo setenforce ®

To make this change permanent - open following config file

```javascript
sudo setenforce 0
```

```javascript
sudo vi /etc/sysconfig/selinux
```

and set


```javascript
SELINUX-disabled
```

![alt text](<Images/Selinux disable.PNG>)

then restart httpd.

```javascript
sudo systemctl start httpd

```

```javascript
sudo systemctl status httpd
```

![alt text](<Images/restart httpd.PNG>)

Change binding address to 0.0.0.0 on MySQL Server

```javascript
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
```

![alt text](<Images/Change binding address in MySQL Server.PNG>)

10. Update the website's configuration to connect to the database (in /var/www/html/functions.php file).

```javascript
sudo vi /var/www/html/functions.php
```

Default View

![alt text](<Images/Update website address to connect to MySQL DB 1.PNG>)

Modified View

![alt text](<Images/Update website address to connect to MySQL DB.PNG>)

On the MySQL Web Server, allow the MySQL/Aurora inbound IP in the inbound rule. Use the custom subnet CIDR IP address linked to your NFS Server and apply it to the customer IP in MySQL inbound rule.

![alt text](<Images/Allow inbound rule for MySQL Web Server.PNG>)

![alt text](<Images/Allow inbound rule for MySQL Web Server 1.PNG>)


On your Web Server, apply tooling-db.sql script to your database using this command mysql -h <databse-private-ip> -u <db-username> -p<db-pasword> < tooling-db.sql


```javascript
sudo yum install mysql
```
![alt text](<Images/Install MySQL on Web 1 Server.PNG>)


On my Web Server, Cd into the tooling directory. The below command is trying to inform us that the file has now been sent as expected.

```javascript
cd tooling
```

```javascript
mysql -h <databse-private-ip> -u <db-username> -p<db-pasword> < tooling-db.sql
```

```javascript
mysql -h 172.31.25.201 -u admin -ppassword1 < tooling-db.sql
```

![alt text](<Images/Cd into tooling on my web server.PNG>)

![alt text](<Images/Cd into tool on my web server.PNG>)


11. Create in MySQL a new admin user with username: myuser and password: password:

```javascript
INSERT INTO 'users' ('id', 'username', 'password', 'email', 'user_type', 'status') VALUES
-> (1, 'myuser', '5f4dcc3b5aa765d61d8327deb882cf99', 'user@mail.com', 'admin', '1');
```

12. Open the website in your browser

```javascript
http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php
```

```javascript
http://18.153.68.12/
```

![alt text](<Images/Screenshot of login to the web server public IP.PNG>)

and make sure you can login into the website with myuser user - Username - admin, password - admin

    the expected outcome is as shown below 

![alt text](<Images/Screenshot of login to the web server public IP after inputing the credentials.PNG>)


## We have just implemented a web solution for a DevOps team using LAMP stack with remote Database and NFS servers.

# Congratulations!

