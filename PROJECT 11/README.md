


# Ansible Automatiomation project



    - What is Ansible and why use IT?

Ansible is an open source, command-line IT automation software application written in Python.

It can configure systems, deploy software, and orchestrate advanced workflows to support application deployment, system updates, and more.

Ansible's main strengths are simplicity and ease of use.

    - Ansible is an automation engine that helps us to automate tasks that are cumbersome, repititive and complex. It makes work easier for the DevOps department.

    - Ansible Client as a Jump Server (Bastion Host)

A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided.

With the current architecture I will be working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet.

That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server - it provides better security and reduces attack surface.

On the diagram below the Virtual Private Network (VPC) is divided into two subnets - Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.


![alt text](<Images/ansible setup.jpg>)


# What is configuration management?

Configuration management is a process for maintaining computer systems, servers, applications, network devices, and other IT components in a desired state. It’s a way to help ensure that a system performs as expected, even after many changes are made over time.

Using configuration management tools, administrators can set up an IT system, such as a server or workstation, then build and maintain other servers and workstations with the same settings. IT teams use configuration assessments and drift analyses to continuously identify systems that have strayed from the desired system state and need to be updated, reconfigured, or patched.


https://www.redhat.com/en/topics/automation/what-is-configuration-management#:~:text=Configuration%20management%20is%20a%20process,in%20a%20desired%2C%20consistent%20state.&text=Managing%20IT%20system%20configurations%20involves,building%20and%20maintaining%20those%20systems


How to Install and Configure Ansible Client to act as a Jump Server/Bastion Host and Create a simple Ansible Playbook to automate server configuration

Prerequistes:

    1. NFS Server
    2.Web Server 1
    3. Web Server 2
    4. Load Balancer
    5. Database Server
    6. Jenkins Server

The following steps are taken to install and configure Ansible Client as a Jump Server/Bastion Host and create a simple Ansible Playbook to automate server configuration:


# Install and configure ansible on ec2 instance

### Install and Configure Ansible on EC2 Instance

    1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

![alt text](<Images/Create an EC2 Instance.PNG>)

![alt text](<Images/Connect via ssh.PNG>)


### Create a new repository called ansible-config-mgt in your GitHub account.

https://github.com/kingsleyezebosi/ansible-config-mgt/new/main?readme=1

![alt text](<Images/Create a new Git Hub Repository.PNG>)

![alt text](<Images/Create a new Git Hub Repository 1.PNG>)


### Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. This server will be used to run playbooks.

Install Ansible on the Jenkins-Ansible server.

https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

```javascript
sudo apt update
```

```javascript
sudo apt install ansible -y
```

![alt text](<Images/sudo apt update.PNG>)

![alt text](<Images/sudo apt install ansible.PNG>)

Confirm that Ansible is installed successfully

```javascript



Check your Ansible Version by running the below command

```javascript
ansible --version
```

![alt text](<Images/Check ansible version.PNG>)


In order to access ansible through the web browser, go to your EC2 instance inbound rule and open TCP port 8080, IPV4 anywhere

![alt text](<Images/Open TCP port 808 in your EC2 instance.PNG>)


It is important to note that Jenkins wont work until Java is installed, hence, we need to install Java language before you can use Jenkins

```javascript
sudo apt install openjdk-17-jre-headless
```

![alt text](<Images/Install Java on Jenkins Server.PNG>)

To check whether Java is installed, run the below command;

```javascript
java
```

![alt text](<Images/View which shows Java is installed.PNG>)


To install Jenkins, run the below commands; REF: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

```javascript
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
```

```javascript
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

```javascript
sudo apt-get update
```

```javascript
sudo apt-get install jenkins
```

![alt text](<Images/Install Jenkins dependencies 1.PNG>)

![alt text](<Images/Install Jenkins dependencies 2.PNG>)

![alt text](<Images/Install Jenkins dependencies 3.PNG>)

```javascript
sudo systemctl status jenkins
```

![alt text](<Images/check Jenkins Status.PNG>)

![alt text](<Images/Jenkins Dashboard.PNG>)

To setup an administrative password for my jenkins, run the below steps;

```javascript
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

![alt text](<Images/sudo default password path.PNG>)

![alt text](<Images/Jenkins Dashboard 1.PNG>)

![alt text](<Images/Jenkins Dashboard 2.PNG>)

![alt text](<Images/Jenkins Dashboard 3.PNG>)

![alt text](<Images/Jenkins Dashboard 4.PNG>)

![alt text](<Images/Jenkins Dashboard 5.PNG>)


# 4. Configure Jenkins build job to archive your repository content every time you change it.

    this will solidify your Jenkins configuration skills acquired in Project 9.

Log into Jenkins.

```javascript
http://public_ip_jenkins_ansible_instance:8080
```

### Create a new Freestyle project ansible in Jenkins and point it to your 'ansible-config-mgt' repository.

Click on the caption "New Item" as shown in the below screenshot

![alt text](<Images/Jenkin Landing Page.PNG>)

Click on the caption "Freestyle project"

![alt text](<Images/Click on Freestyle project.PNG>)

![alt text](<Images/Jenkin configuration.PNG>)

![alt text](<Images/Jenkin configuration 1.PNG>)

![alt text](<Images/Jenkin configuration 2.PNG>)

![alt text](<Images/Jenkin configuration 3.PNG>)

![alt text](<Images/Jenkin configuration 4.PNG>)

![alt text](<Images/Making changes in ansible readme file.PNG>)

## Ansible Build now successful

![alt text](<Images/Ansible build Successful.PNG>)

# Configure a Post-build job to save all (**) files.

## Configure a webhook in GitHub and set the webhook to trigger ansible build.

![alt text](<Images/Github Webhook Config.PNG>)

![alt text](<Images/Github Webhook Created.PNG>)


Enable Proxy Compatibility in Jenkins Security features

![alt text](<Images/Enable Proxy Compatibility.PNG>)


Step 4: Prepare your development environment using Visual Studio Code

    - Download and install VS Code

    - After successfully installing VS Code, configure it to connect to your newly created GitHub repository.

    - Clone down the ansible-config-mgt repository to your local machine.

    - git clone <ansible-config-mgt-repository-link>

Git Clone from Git Hub Repo

![alt text](<Images/Git clone.PNG>)

```javascript
ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/
```

![alt text](<Images/ls to the Var-lib-jenkins-jobs-ansible.PNG>)


- Go into the ansible-config-mgt directory and pull the repo to ensure the directory is up to date.

- `cd ansible-config-mgt && git pull`

![alt text](<Images/Switch Git Branch.png>)

# Step 5: Begin Ansible development and Set up an Ansible Inventory

    - Create a new branch in the ansible-config-mgt repository that will be used for the development of a new feature using the command shown below:

    - git checkout -b prj-145

![alt text](<Images/Git Check out.png>)


# Create a playbooks (i.e. used to store all the playbook files) and inventory (i.e. used to keep your hosts organized) directory.

```javascript
mkdir playbooks inventory
```

![alt text](<Images/Create dir playbook.png>)

- Create your first playbook named common.yml in the playbooks directory.

cd playbooks && touch common.yml

![alt text](<Images/Cd in playbook.png>)

- Create inventory files for each environment (i.e. Development, Staging, Testing and Production) in the inventory directory.

cd .. && cd inventory && touch dev staging uat prod

![alt text](<Images/Cd in Inventory.png>)

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules and tasks in a playbook operate. Since we intend to execute Linux commands on remote hosts and ensure that it is the intended configuration particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

    - To confirm that the key has been added.

```javascript
eval ssh-agent -s
```

```javascript
ssh-add <path-to-private-key>
```

![alt text](<Images/eval ssh agent.png>)

- To ssh into Ansible-jenkins server using ssh agent

ssh -A ubuntu@public-ip

![alt text](<Images/ssh into ansible-jenkins.png>)


Update your inventory/dev.yml file with the code shown below:

```javascript
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
```

![alt text](<Images/Update your inventory-dev.png>)


# Step 8: Create a common playbook

    - It is time to start giving Ansible the instructions on what you need to perform on all servers listen in inventory/dev. In the common.yml playbook, you will write configurations for repeatable, reusable and multi-machine tasks that are common to systems with the infrastructure.

Update your playbooks/common.yml file with the following code:

![alt text](<Images/Update your playbook in Git Hub.png>)

The code above has two plays, the first play is dedicated to the RHEL Servers (i.e. NFS, Database, Web). The task will be performed as the root user hence the use of the become: yes key value pair. Since they are all RHEL Based Servers, the yum module is used to install wireshark and finally the state: latest key-value pair is used to specify that the wireshark installed is the latest version.

The second play is dedicated to the Ubuntu Server (i.e. Load Balancer), it has two tasks: The first task is used to update the server. The update_cahce is similar to apt update and the second task is used to download the latest version of wireshark using the apt module.


# Step 9: Update GIT with the latest code

Now that all of your directories and files live on your local machine, you need to push changes made locally to GitHub. Remember you have been working on a separate branch prj-145, you need to get your branch peer-reviewed and pushed to the main branch. The following steps are taken to achieve this:

    - Use the following commands to check the status of your branch, add files and directories then commit changes and push your branch to GitHub:

    - Go to your ansible-config-mgt repository on GitHub and click on the Compare & pull request button.

![alt text](<Images/Git latest code update.png>)

- Click on the Create pull request button.

![alt text](<Images/Create pull request button.png>)

- Click on the Merge pull request button and confirm

![alt text](<Images/merge pull request button.png>)

    - Head back to your terminal on VS Code, checkout from prj-145 branch into the main and pull down the latest changes using the commands shown below:

    - git checkout main && git pull

Once your code changes appear on the main branch, Jenkins will be triggered to do the ansible job and save all the files (i.e. build artifacts).

![alt text](<Images/Jenkin triggered.png>)

# Step 10: Run the first Ansible test

    - SSH into the Jenkins-Ansible server.

ssh ubuntu@public_ip_address_of_jenkins_ansible

![alt text](<Images/Run first ansible test.png>)


The build artifacts are saved in the /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on the server.

```javascript
cd /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ && ll
```

![alt text](<Images/Build artifacts.png>)


- Run the following command to run the Ansible Playbook.

```javascript
ansible-playbook -i inventory/dev playbooks/common.yml
```

![alt text](<Images/Run ansible playbook.png>)

- Use the Ansible Adhoc command to check if wireshark has been installed on the servers.

```javascript
ansible webservers -i inventory/dev -m command -a "wireshark --version"
```

![alt text](<Images/Check wire shark using ansible adhoc command.png>)


```javascript
ansible nfs,db -i inventory/dev -m command -a "wireshark --version"
```

![alt text](<Images/ansible nfs, db.png>)


```javascript
ansible lb -i inventory/dev -m command -a "wireshark --version"
```

![alt text](<Images/ansible inventory on wire shark.png>)

# SUCCESS !!!

We have just automated our routine tasks by implementing Ansible project!

The architecture looks like this:

![alt text](<Images/Great Job.png>)










































































































































































































