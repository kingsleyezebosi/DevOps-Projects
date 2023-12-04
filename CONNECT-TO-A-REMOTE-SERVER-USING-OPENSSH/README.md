
# CONNECT SECURELY TO A REMOTE SERVER USING OPEN SSH

NOTE: SSH uses an Asymmetric Cipher for encryptions. This means we need 2 keys in a pair, one is a private key while the other is a public key. SSH Listens on Port 22.

Launch your two instances in AWS. One of the instances will be called remote server while the other will be called client.

On the client, run the below command to confirm that the folder where ssh keeps it's data is currently empty.

`ls -latr /home/ubuntu/.ssh`

![Alt text](<Images/ssh folder repo is empty.PNG>)

Also, in order for the client to be able to connect to the remote, the user ubuntu has to exist in the remote server.

`id`

![Alt text](<Images/user ubuntu exists in the remote server.PNG>)

On the client, we will have to generate the ssh key pairs by running the below command.

`ssh-keygen`

![Alt text](<Images/ssh keygen generated on the client machine.PNG>)

To validate that the ssh key pairs has been generated.

`ls -latr /home/ubuntu/.ssh/`

![Alt text](<Images/validate that the ssh keygen has been generated on the client machine.PNG>)

How does the ssh private key looks like.

`cat /home/ubuntu/.ssh/id.rsa`

![Alt text](<Images/open ssh private key.PNG>)

How does the ssh public key looks like. It has an EXT with .pub

`cat /home/ubuntu/.ssh/id.rsa_pub`

![Alt text](<Images/open ssh public key.PNG>)

To show you have a client tool on the client maachine. 

`ssh`

![Alt text](<Images/To show you have a client tool on the client machine.PNG>)

On the remote server, install the openssh command.

`sudo apt install openssh-server -y`

![Alt text](<Images/openssh installed on the remote server.PNG>)

![Alt text](<Images/openssh installed on the remote server.PNG>)

When you check the ssh directory on the remote server, you will find that the 2 keygens should not be present in the remote server, rather it should be found on the client machine.

`ls -latr ~/.ssh`

Remote server

![Alt text](<Images/check the ssh dir on the remote server.PNG>)

Client Machine.

![Alt text](<Images/ssh keygen on the client machine.PNG>)

In other to remote access from the client to the remote server, we will need to copy the public key from the client over to the Remote Server.

To copy the public key from the client, run the below command on the client machine.

`cat ~/.ssh/id_rsa.pub`

Highlight the public key, right click-right and click on copy

![Alt text](<Images/public key on client machine.PNG>)

On the remote server, vi into the authorized key directory to paste the public key. Click on letter I on your keyboard and then paste the public key.

`sudo vi ~/.ssh/authorized_keys`

![Alt text](<Images/vi into the authorized keys in the remote server.PNG>)

Paste the public key in the remote server. To Save is, press esc, shft + :, wq!, then press enter.

![Alt text](<Images/paste the public key in the remote server.PNG>)

To verify the public key was saved, run the below command.

`cat ~/.ssh/authorized_keys`

![Alt text](<Images/validate the public key was saved in the remote server.PNG>)

To ssh from the client to the remote server, we will have to run the below command. The IP is the public IP of your remote server.

`ssh ubuntu@3.95.61.176`

![Alt text](<Images/ssh remote connection successful.PNG>)

In the above screenshot, please note the below information;

The IP 172-31-27-238 - Is the private ip of my client machine.
While the IP 172-31-29-84 - Is the private IP of my remote server.

I created a folder in my remote server called, New_Folder_For_My_Client_Machine.

![Alt text](<Images/test remote connection on remote server. Created a new folder.PNG>)

I could find the folder in my client server.

![Alt text](<Images/Validate folder created from my remote server on the client machine.PNG>)


# THANK YOU!!!























































































































































































































































































































































