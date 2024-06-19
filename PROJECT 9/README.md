
# IMPLEMENTING WORDPRESS WEBSITE WITH LOGIC VOLUME MANAGEMENT (LVM) STORAGE MANAGEMENT - Kingsley

## UNDERSTANDING 3 TIER ARCHITECTURE

![alt text](<Images/3 tier architecture.jpg>)

### SCOPE:

Generally,web, or Mobile solutions are Implemented based on what is called Three-tier Architecture

    - Presentation Layer(PL): This is the user interface such as the client server or browser on your Laptops.

    - Business Layer(BL); This is the backened program that implements bsiness logic. Application or webserver.

    - Data Access or Management Layer(DAL): Thus us the layer for thr computer data storage and data access.. Database server or File system ssuch as FTP server or NFs server.


### Prerequisite;

    - A laptop or Pc to serve as a client

    - An EC2 Linux Server as web server (This is where you will install Wordpress)

    - An EC2 Linux Server as a database(DB) server.

### Implementing LVM on Linux servers (Web and Database servers)

###### Step 1 - PREPARE A WEB SERVER.

    -Launch an EC2 instance the will serve as "Web Server"

    - Create 3 Volumes in the same Az as your Web server EC", each of 10gigs

    - Launching an EC2 Instance called Web-Server with a Redhat OS


![alt text](<Images/Webserver Volume EC2.PNG>)

###### Create 3 volumes , 10Gb each.

![alt text](<Images/Create Volume.PNG>)

![alt text](<Images/Volumes created.PNG>)


- To attach each Volume one by one to the Webserver EC2 Instance

- Click on the Volume and right click to select the attach option.

- Select the Web-server EC2 instance and click attach

![alt text](<Images/Webserver Volume EC2.PNG>)

- Creating a volume of 10gibs

![alt text](<Images/Create Volume.PNG>)

![alt text](<Images/Volumes created.PNG>)

- To attach each Volume one by one to the Webserver EC2 Instance

- Click on the Volume and right click to select the attach option.

- Select the Web-server EC2 instance and click attach

![alt text](<Images/Attach Volume.PNG>)

# Step 2

To connect the EC2 Instance to the Terminal via ssh client

Cd into downloads and paste the key ssh -i "webservvol.pem" ec2-user@ec2-16-170-255-94.eu-north-1.compute.amazonaws.com

![alt text](<Images/cd in the EC2 instance linked to the Volume.PNG>)

- To inspect what block device is attched to the server

```javascript
lsblk
```

![alt text](<Images/Check what block device attched to the server.PNG>)

- To see all mount and free space on the web-server

```javascript
df -h
```

![alt text](<Images/See all mount and free space.PNG>)

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

Run a new entry by entering n and click the number of partition in this case 1

Click yes to complete the process

![alt text](<Images/Creating a single partition 1.PNG>)

![alt text](<Images/Creating a single partition 2.PNG>)


- To view the newly configured partition on each of the 3 disks

```javascript
lsblk 
```

![alt text](<Images/View configured partitioned disk.PNG>)

-  Install lvm2 package using the below command;

```javascript
sudo yum install lvm2
```

![alt text](<Images/Install lvms package 1.PNG>)

![alt text](<Images/Install lvms package 2.PNG>)

Run the below command to check for available partitions

```javascript
sudo lvmdiskscan
```

![alt text](<Images/To check available partitions.PNG>)

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

![alt text](<Images/Mark each of the three disks as physical Volumes to be used by LVM 1.PNG>)


- To verify that the physical volume has been created successfully

```javascript
sudo pvs
```

![alt text](<Images/Verify PVS has been created.PNG>)

- To add all the three Physical volumes(Pvs) to a Volume Group (VG). Name the VG webdata-vg

```javascript
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```

![alt text](<Images/Add 3 vol to VG.PNG>)

- To Verify that the VG has been successfully Created

```javascript
sudo vgs
```

![alt text](<Images/Verify the VG has been created.PNG>)

- To create 2 logical volumes.apps-lv(Use half of the Pv size), and logs-lv(Use the remaining space of the PV)

- Note; Apps-lv will be used to store data for the website while,logs-lv will be used to store data for logs.

    ```javascrip
    sudo lvcreate -n apps-lv -L 14G webdata-vg
    ```

    ```javascript
    sudo lvcreate -n logs-lv -L 14G webdata-vg
    ```

![alt text](<Images/Create 2 logical app and log vol.PNG>)

- To verify the logic Volumes has been created successfully.

```javascript
sudo lvs
```

![alt text](<Images/Verify the logical app and log vol has been created.PNG>)

- To verify the entire set-up

```javascript
sudo vgdisplay -v #view complete setup - VG, PV, and LV
```

![alt text](<Images/Verify the entire setup.PNG>)

```javascript
sudo lsblk 
```

![alt text](<Images/Verify the entire setup 1.PNG>)

- To format the logical app and log Volumes with ext4 file system using mkfs.ext4

```javascript
sudo mkfs -t ext4 /dev/webdata-vg/apps-lv
```

```javascript
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![alt text](<Images/Format the logical app and log vol.PNG>)

- To create /var/www/html directory to store websites file

```javascript
sudo mkdir -p /var/www/html
```

- To create /home/recovery/logs to store back up of the logs data

```javascript
sudo mkdir -p /home/recovery/logs
```

- To mount /var/www/html on apps -lv logical volume

```javascript
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

![alt text](<Images/Create html dir and mount logical vol on it.PNG>)

- To back up files in the log directory /var/log into home/recovery/logs using the rsync utility script, please run the below;

```javascript
sudo rsync -av /var/log/. /home/recovery/logs/
```

![alt text](<Images/Back up files in the log directory.PNG>)

- To mount var/log on logs-lv logical volume

```javascript
sudo mount /dev/webdata-vg/logs-lv /var/log
```

- To restore log files back into /var/log directory

```javascript
sudo rsync -av /home/recovery/logs/log/. /var/log
```

![alt text](<Images/Mount log on logs logical volume.PNG>)

- To update /etc/fstab so that the mount configuration will persist after restart. Copy the UUID of the device to upate the /etc/fstab

```javascript
sudo blkid
```

apps : UUID=203567a2-3c09-4fbc-adee-9dde15c3903f /var/www/html ext4 defaults 0 0

logs : UUID=b7b9ba8d-87db-42fa-a663-32ced16102d2 /var/log ext4 defaults 0 0

xfs : 497ad222-04fa-453f-b110-ba8001d38788

![alt text](<Images/Copy UUID of Apps & Logs & XFS.PNG>)

Run the below command and then past the UUIDs.

```javascript
sudo vi /etc/fstab
```

![alt text](<Images/Paste the UUIDs inside the Vi file.PNG>)

- To test the configuration and reload the Daemon

```javascript
sudo mount -a 
```

AND

```javascript
sudo systemctl daemon-reload
```

![alt text](<Images/Mount and Reload after pasting the UUIDs in the file.PNG>)

- To verify the set up is running well

```javascript
df -h
```

![alt text](<Images/Verify the file is running smoothly.PNG>)


# HEADING - INSTALLING WORDPRESS AND CONFIGURING IT TO USE MYSQL DATABASE

# STEP-2 PREPARING THE DATABASE SERVER

Repeating the same step above, BUt instead of apps-lv create db-lv and mount it into directory /db instead of /var/www/html

Launch an EC2 instance DB-serveron redhart OS

![alt text](<Images/DB server Instance.PNG>)

Create and attach 3-volumes to the DB-Server

![alt text](<Images/Create volume for the DB Server.PNG>)

![alt text](<Images/Attach Volume to DB Server.PNG>)

Connect to the DB-Server via ssh. Then, to see the newly created devices

```javascript
lsblk
```

![alt text](<Images/lsblk to see the attached vols.PNG>)

![alt text](<Images/lsblk to see the attached vols 1.PNG>)

To create a single partition on each of the 3 disks

```javascript
sudo gdisk /dev/nvme1n1
```

```javascript
sudo gdisk /dev/nvme2n1
```

```javascript
sudo gdisk /dev/nvme3n1
```

Run a new entry by entering n and click the number of partition in this case 1

Click yes to complete the process

![alt text](<Images/Disk partition 1.PNG>)

![alt text](<Images/Disk partition 2.PNG>)

![alt text](<Images/Disk partition 3.PNG>)

To view the newly configured partition on each of the 3 disks

```javascript
lsbk 
```

![alt text](<Images/View the newly partitioned disk.PNG>)

To install the lvm2 package

```javascript
sudo yum install lvm2
```

![alt text](<Images/Install lvms package for the DB Server.PNG>)

Run the below command to check for available partitions

```javascript
sudo lvmdiskscan
```

![alt text](<Images/Check available partition after LVMs 1.PNG>)

![alt text](<Images/Check available partition after LVMs.PNG>)


To mark each of the three disks as physical Volumes(Pvs) to be used by LVM, run the below commands

```javascript
sudo pvcreate /dev/nvme1n1p1 
```

```javascript
sudo pvcreate /dev/nvme2n1p1 
```

```javascript
sudo pvcreate /dev/nvme3n1p1
```

![alt text](<Images/Mark 3 disks to physical vol for DB Server.PNG>)


To verify that the physical volume has been created successfully

```javascript
sudo pvs
```

![alt text](<Images/Verify vols are created for DB Server.PNG>)

To add all the three Physical volumes(Pvs) to a Volume Group (VG). Name the VG webdata-vg

```javascript
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```

To Verify that the VG has been successfully Created

```javascript
sudo vgs
```

![alt text](<Images/Add 3 vol to VG & Verify for DB Server.PNG>)

To create 2 logical volumes.apps-lv(Use half of the Pv size), and logs-lv(Use the remaining space of the PV)

Note; Apps-lv will be used to store data for the website while,logs-lv will be used to store data for logs.

```javascript
sudo lvcreate -n db-lv -L 14G webdata-vg
```

```javascript
sudo lvcreate -n logs-lv -L 14G webdata-vg
```

To verify the logic Volumes has been created successfully.

```javascript
sudo lvs
```

![alt text](<Images/Create 2 logical db and log vol & verify.PNG>)


To format the logical app and log Volumes with ext4 file system using mkfs.ext4

```javascript
sudo mkfs -t ext4 /dev/webdata-vg/db-lv
```

```javascript
sudo mkfs -t ext4 /dev/webdata-vg/logs-lv
```

![alt text](<Images/Format the logical db and log vol.PNG>)

To create /db directory to store websites file. Run the below command. Mount it to /db (instead of var/www/html)

```javascript
sudo mkdir -p /db
```

To create /home/recovery/logs to store back up of the logs data. Run the below command

```javascript
sudo mkdir -p /home/recovery/logs
```

![alt text](<Images/Create a db dir to store files.PNG>)

To mount /db on db-lv logical volume

```javascript
sudo mount /dev/webdata-vg/db-lv /db
```

![alt text](<Images/Mount the db on the logical vol.PNG>)


To back up files in the log directory /var/log into home/recovery/logs using the rsync utility

```javascript
sudo rsync -av /var/log/. /home/recovery/logs/
```

![alt text](<Images/Resync the var log and recovery logs.PNG>)


To update /etc/fstab so that the mount configuration will persist after restart. Copy the UUID of the device to upate the /etc/fstab

```javascript
sudo blkid
```

db: UUID=c14c5f35-dc85-4137-ba70-0df5fe899d34 /var/www/html ext4 defaults 0 0

logs: UUID=efb91f6f-30ba-4f54-947a-b73abf1706ec /var/log ext4 defaults 0 0

xfs: d0be04e8-fc37-4928-af70-fc3d6595ef71

![alt text](<Images/To check the UUIDs in the DB Server.PNG>)


Run the below command and then past the UUIDs.

```javascript
sudo vi /etc/fstab
```

![alt text](<Images/vi in the config file to paste the UUIDs.PNG>)

To test the configuration and reload the Daemon

```javascript
sudo mount -a 
```

AND

```javascript
sudo systemctl daemon-reload
```

![alt text](<Images/sudo mount and reload to ensure the setup is correct..PNG>)

To verify the set up is running well


```javascript
df -h
```

![alt text](<Images/Verify all is runing well for db.PNG>)


# STEP-3 INSTALLING WORDPRESS ON YOUR WEB SERVER EC2 INSTANCE

Before we install Wordpress, we would first update the reprository by running the below command

```javascript
sudo yum -y update
```

![alt text](<Images/Update Linux with sudo yum -y.PNG>)

- To install Apache, wget and its dependencies

```javascript
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```

![alt text](<Images/Install apache wget and dependencies 1.PNG>)

![alt text](<Images/Install apache wget and dependencies 2.PNG>)

![alt text](<Images/Install apache wget and dependencies 3.PNG>)

- To start apache

```javascript
sudo systemctl start httpd 
```

AND Run 

```javascript
sudo systemctl enable httpd
```

![alt text](<Images/enable & Start httpd.PNG>)

- To install PHP and its dependencies, run the below command;

```javascript
sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

```javascript
sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

```javascript
sudo yum module list php
```

```javascript
sudo yum module reset php
```

```javascript
sudo yum module enable php:remi-7.4
```

```javascript
sudo yum install php php-opcache php-gd php-curl php-mysqlnd
```

```javascript
sudo systemctl start php-fpm
```

```javascript
sudo systemctl enable php-fpm
```
```javascript
sudo systemctl enable php-fpm
setsebool -P httpd_execmem 1
```

![alt text](<Images/Install PHP and it's dependencies 1.PNG>)

![alt text](<Images/Install PHP and it's dependencies.PNG>)

![alt text](<Images/Install PHP and it's dependencies 2.PNG>)

![alt text](<Images/Install PHP and it's dependencies 3.PNG>)

![alt text](<Images/Install PHP and it's dependencies 4.PNG>)


-  Restart Apache

```javascript
sudo systemctl enable httpd
```

![alt text](<Images/Enable httpd.PNG>)

- To downlaod Wordpress and copy wordpress to var/www/html, run the below command;

```javascript
mkdir wordpress
```

```javascript
cd   wordpress
```

```javascript
sudo wget http://wordpress.org/latest.tar.gz
```

```javascript
sudo tar xzvf latest.tar.gz
```

```javascript
sudo rm -rf latest.tar.gz
```

```javascript
sudo cp wordpress/wp-config-sample.php wordpress/wp-config.php
```

```javascript
sudo cp -R wordpress /var/www/html/
```

![alt text](<Images/Download Wordpress & Copy to var-www-html.PNG>)

![alt text](<Images/Download Wordpress & Copy to var-www-html 1.PNG>)


-  To configure SElinux Policies

```javascript
 sudo chown -R apache:apache /var/www/html/wordpress
 ```

 ```javascript
 sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
 ```

 ```javascript
 sudo setsebool -P httpd_can_network_connect=1
```

![alt text](<Images/Configure SElinux Policies.PNG>)


# STEP 4 - INSTALLING MYSQL ON YOUR DB SERVER EC2 

- To update and Install mysql-server

```javascript
sudo yum update
```

![alt text](<Images/sudo yum update on DB Server.PNG>)

![alt text](<Images/sudo yum update on DB Server 1.PNG>)

```javascript
sudo yum install mysql-server
```

![alt text](<Images/sudo yum install mysql db server 1.PNG>)

![alt text](<Images/sudo yum install mysql db server 2.PNG>)

- To verify if the set-up is running


```javascript
sudo systemctl restart mysqld
```

```javascript
sudo systemctl status mysqld
```

![alt text](<Images/Verify that MySQL is running.PNG>)


# STEP 5 - CONFIGURE DB TO WORK WITH WORDPRESS


```javascript
sudo mysql
```

```javascript
CREATE DATABASE wordpress;
CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';
GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

```javascript
CREATE DATABASE wordpress;
CREATE USER `myuser`@`172.31.38.237` IDENTIFIED BY 'mypass1';
GRANT ALL ON wordpress.* TO 'myuser'@'172.31.38.237';
FLUSH PRIVILEGES;
SHOW DATABASES;
exit
```

![alt text](<Images/Configure db to work with Wordpress.PNG>)

# STEP 6 - CONFIGURE WORDPRESS TO CONNECT TO REMOTE DATABASE

- Open MYSQL port 3306 on DB-server and access it only from web-servers IP address

- In the inbound rule configure source as /32

![alt text](<Images/Open inbound rule to connect to Remote DB.PNG>)

To Install MySQL client on the web server and test that you can connect from Web-server to DB-server using mysql-client

```javascript
sudo yum install mysql
```

On the Database Server, Create an admin user in mysql using the below parameter. This will be used to create a remote connection through the web server.

```javascript
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```
Note: Username should be your desired name; host - replace host with the Subnet cidr of the webserver

```javascript
CREATE USER 'admin'@'172.31.32.0/20' IDENTIFIED BY 'password1';
```

To remote login to the DB server from the web server, please run the below command on the web server

```javascript
sudo mysql -u admin -p -h <DB-Server-Private-IP-address>
```

```javascript
sudo yum install mysql
sudo mysql -u admin -p -h 172.31.40.35
```

![alt text](<Images/Create a user in DB Server for remote connection through the web server.PNG>)

verify you can execute SHOW DATABSES;

change permissions and configurations so Apache could use wordpress.

enable TCP port 80 in inbound rules configuration for Web server

![alt text](<Images/Enable TCP Port 80 on the web server vol.PNG>)

On the web server, vi into the wp-config file by runing the below command

```javascript
sudo vi /var/www/html/wordpress/wp-config.php
```

Note - Replace the following with the real values. database_name_here with the wordpress, username_here with admin, password_here with the password of the admin, localhost with the private ip of the database server.

It's original view

![alt text](<Images/Vi into the wp-config file.PNG>)

It's modified view which is how it should look.

![alt text](<Images/Vi into the wp-config file 1.PNG>)

Now, try to access from your browser link to the wordpress using your web server ip.

```javascript
http://<web-server-public-ip-address>/wordpress/
```

```javascript
http://13.60.20.166/wordpress
```

![alt text](<Images/Access wordpress using web server ip 1.PNG>)

![alt text](<Images/Access wordpress using web server ip 2.PNG>)

![alt text](<Images/Access wordpress using web server ip 3.PNG>)


# SUCCESS

# CONGRATULATIONS






























































































