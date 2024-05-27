
# HOW TO SECURELY UPLOAD AND DOWNLOAD FILES REMOTELY SFTP - SCP

#### I have a client machine and a remote server. My goal is to upload the folder directory tree structure I have present in my client machine to my remote server.

The below command will show you the entire folder directory structure. 

`tree DevOps_Materials`

![Alt text](<Images/tree dir structure.PNG>)

Create an html folder directory inside the var/www/ path.

`sudo mkdir -p /var/www/html`

And then, ls /var/www/

![Alt text](<Images/ls into the var and www path.PNG>)

On the client machine, remotely login to the remote server

`sftp public ip of the remote server` or `sftp ubuntu@public ip of the remote server`

![Alt text](<Images/remote connection to the server established.PNG>)

On the client machine, cd into the /var/www/html path

`cd /var/www/html`

![Alt text](<Images/cd into the var, www and html path.PNG>)

Initiate the folder directory transfer from the client machine to the remote server

`put -r DevOps_Materials`

And I got an error saying "Entering DevOps_Materials/ stat remote: No such file or directory"

![Alt text](<Images/put -r error message dir does not exist.PNG>)

To resolve this, I would have to create the folder "DevOps_Materials" in the remote server.

`sudo mkdir -p /var/www/html/DevOps_Materials`

![Alt text](<Images/create the DevOps folder in the remote server.PNG>)

Return to the client and reinitiate the upload again. You will get an error saying "Couldn't setstat on "/var/www/html/DevOps_Materials" : Permission Denied". Return back to the remote server and run the below command.

`sudo chown ubuntu:ubuntu /var/www/html/DevOps_Materials`

![Alt text](<Images/permission denied when uploading.PNG>)

Return to the client server and re-upload. The will now be successful.

`put -r DevOps_Materials`

![Alt text](<Images/upload successful.PNG>)

Once it is successful, reboot the remote server.

`sudo systemctl reboot`

Once the reboot is completed, return to the remote server and run the below command to see if the number of items tally with what you have on the client.

![Alt text](<Images/confirm upload on the remote server.PNG>)

![Alt text](<Images/confirm upload on the remote server 1.PNG>)

![Alt text](<Images/confirm upload on the remote server 2.PNG>)


# HOW TO SECURELY DOWNLOAD A FOLDER DIRECTORY STRUCTURE FROM MY REMOTE SERVER TO THE CLIENT MACHINE.

On the remote server, let's rename the Tools folder inside the DevOps_Material parent folder.

`mv Tools/ to_be_downloaded`

![Alt text](<Images/Tools folder renamed.PNG>)

Confirm that you're still connected to the remote server on the client machine by running the below command.

`pwd` 

`ls DevOps_Materials`

This would show you the folder we want to download. Folder name is "to_be_downloaded".

![Alt text](<Images/confirm still connected to the remote server from the client.PNG>)

On the client machine, run the below command to download the folder.

`get -r DevOps_Materials/to_be_downloaded`

![Alt text](<Images/retrieve downloaded folder.PNG>)

Once the folder is retrieved from the remote server to the client machine, exit the remote connection.

`exit`

On the client machine, run the below command

`ls -ltr`

![Alt text](<Images/confirm folder was downloaded.PNG>)

On the client server, let's rename the "to_be_donwloaded" folder to "scp_upload".

`mv to_be_donwload/ scp_upload`

![Alt text](<Images/rename folder to scp_upload.PNG>)

![Alt text](<Images/rename folder to scp_upload 1.PNG>)

We will have to upload the "sc_upload" folder from the client machine to the remote server in the /tmp directory.

On the remote server, cd into the /tmp/ directory

`cd /tmp`

![Alt text](<Images/cd to the tmp directory.PNG>)

On the client machine, please run the below command.

`scp -r scp_upload/ ubuntu@18.212.79.187 (the public ip address of the remote server):/tmp`

![Alt text](<Images/scp uploaded to the remote server from the client machine..PNG>)

To verify that the upload was successful, run the fllowing commands below on the remote server.

cd into the tmp location on the remote server.

`cd /tmp/`

Run the below command to validate that the directory uploads was successful.

`ls -ltr /tmp/scp_upload`

![Alt text](<Images/scp upload validation on the remote server.PNG>)

## USING THE SCP PROTOCOL TO DONWLOAD A DIRECTORY FROM THE REMOTE SERVER TO THE CLIENT MACHINE. 

On the remote server, rename thr scp_upload folder found in the tmp directory to scp_download.

`mv scp_upload/ scp_download`

![Alt text](<Images/rename scp upload to scp download.PNG>)

![Alt text](<Images/rename scp upload to scp download 1.PNG>)

Run the below command on the remote server.

`ls -ltr`

`ls -ltr /tmp/scp_downloads`

![Alt text](<Images/ls -ltr scp download on the remote server.PNG>)

On the client server, run the below command to comfirm if te scp_download folder is there, although it should not be there.

`ls `

![Alt text](<Images/The scp_download folder is not present in the client machine.PNG>)

To download the scp_download folder from the remote server to the client machine, please run the below command,

Check the pwd on your client server.

`pwd`

![Alt text](<Images/run pwd on the client to check the location..PNG>)

Run the below command to check the absolute part for your home directory. Note: The absolute gives you the entire directory path while the relative path give you the precised directory path. 

`ls -ltr /home/ubuntu`

![Alt text](<Images/check the absolute path using ls -ltr.PNG>)

Run the below command to download the directory to your client machine using the SCP Protocol.

`scp -r ubuntu@18.212.79.187:/tmp/scp_download /home/ubuntu/`

![Alt text](<Images/file download on the client machine using scp protocol.PNG>)

Run the below command on the client machine to validate the download.

ls -ltr

![Alt text](<Images/validate the scp download on the client machine.PNG>)











































































































