
# CREATE A USER AND ASSIGN ADMINISTRATIVE PRIVILEDGES TO THE USER

### Create a user called "kelvin"

`sudo useradd kelvin`

![Alt text](<Images/Create a user.PNG>)

### Verify that the user has been created

`id kelvin`

![Alt text](<Images/Verify user is created.PNG>)

### create a password for the user

`sudo passwd kelvin`

![Alt text](<Images/Create a user password.PNG>)

### Sudo into the newly created user.

`sudo su kelvin`

![Alt text](<Images/sudo into kelvin.PNG>)

### Confirm that kelvin does not have any admin privilege prior to now.

`sudo ls /`

![Alt text](<Images/kelvin has no admin right.PNG>)

### Exit from kelvin directory and return to the previous admin directory so you can assign the privileges.

`exit`

### ls into the sudoers file to confirm if the file indeed exists. The sudoers file exist in other to allow you have an administrative privileges.

`ls -latr /etc/sudoers`

![Alt text](<Images/check if sudoers file exist.PNG>)

### Vi into the sudoers file. Under the User Priviledge Specification, add kelvin in the same line, press esc, type shift :, type wq!, then press enter to Save.

`sudo vi /etc/sudoers`

![Alt text](<Images/vi into the sudoers file 1.PNG>)

![Alt text](<Images/add kelvin under the User Priviledge Specification.PNG>)

![Alt text](<Images/save the sudoer priviledge file.PNG>)

### Verify the configuration

`sudo visudo -c`

![Alt text](<Images/sudo visudo command.PNG>)

### Now, Sudo into the newly created user.

`sudo su kelvin`

![Alt text](<Images/sudo into kelvin.PNG>)

### ls into the root directory

`sudo ls /`

![Alt text](<Images/kelvin now has admin rights.PNG>)

## Exit from kelvin directory to the default directory. 

### Delete kelvin from the User Priviledge Specification and add him to the default administrative group, then confirm if he will have the admin prviledge. 

### vi into the sudoers file

`sudo vi /etc/sudoers`

![Alt text](<Images/vi into the sudoers file 1.PNG>)

### Delete kelvin's name by double tapping the letter d while the mouse cursor is blinking beside kelvin name.

![Alt text](<Images/mouse cursor beside kelvin name.PNG>)

### Admin priviledges removed.

![Alt text](<Images/admin rights removed.PNG>)

### Kelvin now has no right.

`sudo ls /`

![Alt text](<Images/kelvin now has no admin rights.PNG>)

`Exist`

### vi into the sudoers file

`sudo vi /etc/sudoers`

Under the name tagged "Members of the admin group may gain root privileges", add a group called %devops ALL=(ALL) ALL in the same line, press esc, type shift :, and wq!, then press enter to Save.

![Alt text](<Images/add devops group to the root default group.PNG>)

### Create the devops group

`sudo groupadd devops`

![Alt text](<Images/sudo groupadd devops.PNG>)

### Add kelvin to the recently created devops group.

`sudo usermod -a -G devops kelvin`

![Alt text](<Images/add kelvin to the devops geoup.PNG>)

### Id kelvin to check the group he belongs to.

`id kelvin`

![Alt text](<Images/id into kelvin to confirm the group he belongs to.PNG>)

### Run the Visudo to confirm everything is okay.

`sudo visudo -c`

![Alt text](<Images/visudo command.PNG>)

### Now, Sudo into the newly created user.

`sudo su kelvin`

![Alt text](<Images/sudo into kelvin.PNG>)

### ls into the root directory, and you will see that Kevin now have administrative privileges.

`sudo ls /`

![Alt text](<Images/kelvin now has admin rights afterwards.PNG>)


# THANK YOU!!!