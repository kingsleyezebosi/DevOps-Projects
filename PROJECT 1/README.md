## LinuxPracticeProject
## File Manipulation
## 1. sudo command
sudo: Shot for superuser do, this is used to perform tasks that requires administartive or root permissions
```javascript
sudo apt update
```
![Alt text](<images/sudo command.PNG>)

## 2. pwd command
pwd means "Print Working Directory". This command is used to find the path of your current /present working directory. 
```javascript
pwd
```
```javascript
pwd [-L]
```
```javascript
pwd [-P]
```
```javascript
pwd [-LP]
```
![Alt text](<images/pwd command.PNG>)

## 3. cd command
```javascript
cd CommandLinux
```
```javascript
cd ~
```
```javascript
cd -
```
```javascript
cd ..
```
![Alt text](<images/cd command.PNG>)

## 4. ls command
```javascript
ls CommandLinux
```
```javascript
ls -R
```
```javascript
ls -a
```
```javascript
ls -lh
```
![Alt text](<images/ls command.PNG>)

![Alt text](<images/ls command 2.PNG>)

## 5. cat command
```javascript
cat sqlite_commands.sh
```
```javascript
cat filename1.txt filename2.txt > filename3.txt
```
```javascript
tac filename3.txt
```
![Alt text](<images/cat command.PNG>)

## 6. cp command
```javascript
cp sqlite_commands.sh /home/kingsley/unixcommands/
```
```javascript
cp filename1.txt filename2.txt filename3.txt /home/kingsley/Documents/
```
![Alt text](<images/cp command.PNG>)

## 7. mv command
```javascript
mv sqlite_commands.txt /home/kingsley/unixcommands
```
```javascript
mv DevOps /home/kingsley/Downloads/
```
![Alt text](<images/mv command.PNG>)

## 8. mkdir command
```javascript
mkdir Album
```
```javascript
mkdir Album/List
```
![Alt text](<images/mkdir command.PNG>)

## 9. rmdir command:
This command is used to permanently delete an empty directory.
```javascript
rmdir Album/List
```
![Alt text](<images/rmdir command.PNG>)

## 10. rm command:
This command is used to delete files within a directory.
```javascript
rm Linux
```
```javascript
rm filename1.txt filename.txt2 filename3.txt
```
![Alt text](<images/rm command.PNG>)

## 11. touch command:
```javascript
touch Linux
```
![Alt text](<images/touch command.PNG>)

## 12. locate command:

```javascript
touch Linux
```
![Alt text](<images/locate command 1.PNG>)

![Alt text](<images/ls command 2.PNG>)

## 13. find command:
This command is used to find a file or folder within a directory. 
```javascript
find /home -name CommandLinux
```
![Alt text](<images/find command.PNG>)

## 14. grep command:
This command is used to find a word by searching through all the texts in a specific file.
```javascript
grep values CommandLinux
```
![Alt text](<images/grep command.PNG>)

## 15. df command:
This command is used to report the system disk space usage.
```javascript
df -h
```
![Alt text](<images/df command.PNG>)

## 16. du command:
This command is used to check how much space a file or a directory takes up.
```javascript
du /home/kingsley/CommandLinux
```
![Alt text](<images/du command.PNG>)

## 17. head command:
This command allows you to view the first 10 lines in a text.
```javascript
head DevOps
```
![Alt text](<images/head command.PNG>)

## 18. tail command:
This command allows you to view the last 10 lines of a file. It shows whether a file has a new data or read error messages.
```javascript
tail DevOps
```
![Alt text](<images/tail command.PNG>)

## 19. diff command:
This command is used to compare two contents of a file line by line. After analyzing them, it will display the part that do not match.
```javascript
diff DevOps Linux
```
```javascript
diff -c DevOps Linux
```
![Alt text](<images/diff command 1.PNG>)

![Alt text](<images/diff command 2.PNG>)

![Alt text](<images/diff command 3.PNG>)

## 20. tar command:
This command is used to archive multiple files into a TAR file. A common Linux format similar to ZIP.
```javascript
tar -cvf filename1.txt.tar /home/kingsley
```
![Alt text](<images/tar command.PNG>)

![Alt text](<images/tar command 2.PNG>)

## File Permissions and Ownership
## 21. chmod command:
This is a command that modifies a file or directory's read, write and execute permissions.Each file in Linux is associated with 3 user classes - Owner, group member, and others.
```javascript
ls -ltr
```
```javascript
chmod 777 DevOps  
```
```javascript
chmod 777 LinuxCommand
```
![Alt text](<images/chmod command.PNG>)

## 22. chown command:
This is used to change the ownership of a file, directory or symbollic link to a specified username.
```javascript
chown kingsley Linux
```
![Alt text](<images/chown command.PNG>)

## 23. jobs command:
This command will display all the running processes along with their statuses. This command is only available in csh, bash, tcsh, and ksh shells.
```javascript
job jobID
```
![Alt text](<images/job command.PNG>)

## 24. kill command:
This command is used to termite an unresponsive program manually by identifying the application's PID number. This will signal misbahaving applications and instruct them to close their processes.
To check their PID Number, please run the command below.
```javascript
ps ux
```
To terminate the program, please type the below command.
```javascript
kill SIGKILL PID
```
![Alt text](<images/kill command 1.PNG>)

![Alt text](<images/kill command 2.PNG>)

## 25. ping command:
This command is used to check if a network or server is reachable. This is mostly used to troubleshoot connectivity issues.
```javascript
ping google.com
```
![Alt text](<images/ping command.PNG>)

## 26. wget command:
This command allows you to download files from the internet using the wget command. This works without hindering any running processes. 
```javascript
wget https://wordpress.org/latest.zip 
```
![Alt text](<images/wget command.PNG>)

## 27. uname command:
This is either called uname or unix name. A command that prints detailed information about your Linux system and hardware, inclusive of the machine name, Operating System, and Kernel.
```javascript
uname -a
```
```javascript
uname -s
```
```javascript
uname -n
```
![Alt text](<images/uname command.PNG>)

## 28. top command:
This command is used to display all running processes and a dynamic real-time view of the current system.
```javascript
top
```
![Alt text](<images/top command.PNG>)

## 29. history command:
With history, the system will list up to 500 previously executed commands allowing you to reuse them without re-entering.
```javascript
history
```
![Alt text](<images/history command.PNG>)

## 30. man command.
this command will provide the user manual of any command or utilities you can run in the terminal including name, description and options.
```javascript
man ls
```
```javascript
man 2 ls
```
![Alt text](<images/man command 1.PNG>)

![Alt text](<images/man command 2.PNG>)

## 31. echo command.
This command is a built in utility that displays a line of text or string using the standard output.
```javascript
echo "How are you?"
```
```javascript
echo \A DevOps
```
```javascript
echo -e DevOps
```
```javascript
echo -n DevOps
```
![Alt text](<images/echo command.PNG>)

## 32. zip, unzip commands:
This command will compress files and directories into a zip file, to archive or reduce disk space usage.
```javascript
zip archive.zip blog
```
```javascript
zip archive.zip text
```
```javascript
unzip archive.zip
```
## 33. hostname command:
This command will show the system's hostname or ip address
```javascript
hostname
```
```javascript
hostname -i
```
```javascript
hostname -A
```
```javascript
hostname -alias
```
![Alt text](<images/zip & unzip command.PNG>)

## 34. useradd, userdel command
The useradd command will create a new account the passwd command allows you to add a password
```javascript
sudo useradd Kelvin
```
Before I proceed to create a password for Kelvin, I have to type in sudo -i, and then type, id Kelvin
```javascript
sudo passwd Kelvin 
```
To delete the user 
```javascript
userdel John
```
To return back to the previous user account, run the "exit" command, then run "logout"
![Alt text](<images/useradd passwd userdel command 1.PNG>)

![Alt text](<images/useradd passwd userdel command 2.PNG>)

## 35. apt-get command
This command handles advanced package tools (APT) in Linux. It lets you retrive information and bundles from authenticated sources.
```javascript
apt-get update
```
```javascript
sudo apt-get update
```
![Alt text](<images/apt-get update command 1.PNG>)

![Alt text](<images/apt-get update command 2.PNG>)

## 36. nano, vi, jed commands:
The nano command denotes keywords and can work with most languages.
```javascript
nano DevOps
```
The vi command works in two modes insert and command: this can edit and create a text file perform operations such as copying, saving, openig and pasting a file.

```javascript
vi Linux
```
The jed has a dropdown interface that allows users to perform tasks without entering keyboard combinations or commands.

```javascript
jed Linux
```
![Alt text](<images/nano command 1.PNG>)

## 37. alias, unalias command
This command allows you to create a shortcut with the same functionality as a command, file name or text.
```javascript
alias dv='DevOps'
```
The unalias command deletes an existing alias
```javascript
unalias dv
```
![Alt text](<images/alias unalias command.PNG>)

## 38. su command:
This command allows you to switch user or run a program as a different user. It is beneficial for accessing the system through SSH or GUI display when root user is unavailable. 
```javascript
su
```
![Alt text](<images/su command.PNG>)

## 39. htop command:
This command monitors system resources and server processes htop command has additional features and improvements than the top command. Before you can run the below command, you have to install the htop service using the command "sudo snap install htop" or "sudo apt install htop". 
```javascript
htop -d
```
```javascript
htop -C
```
```javascript
htop -h
```
![Alt text](<images/htop command.PNG>)

## 40. ps command:
This command produces a snapshot of all the processes on your system.
```javascript
ps -T
```
```javascript
ps -u
```
```javascript
ps -A
```
```javascript
ps -e
```
![Alt text](<images/ps command 1.PNG>)

![Alt text](<images/ps command 2.PNG>)

## THANK YOU!










































