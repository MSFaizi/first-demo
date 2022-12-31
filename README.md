# first-demo Project 
follow the below steps
![image](https://user-images.githubusercontent.com/102685509/210142066-f6490af7-19c7-415d-b28c-b43c37e62896.png)

Let us begin with the steps as in the picture above \
Step-1: Create four servers in AWS and mention the name as shown in below pic

![image](https://user-images.githubusercontent.com/102685509/210143860-27d06b2f-b26d-46bd-befe-fb8572db40af.png)

Step-2: Take SSH connection of these all 4 servers Via Putty \
Step-3:  Set the name of all servers in Putty with their proper name as given in AWS, It will help to recognize server with their name, run bellow command 
- ec2-user
- sudo su
- hostnamectl set-hostname [server name]
- exit
- sudo su 
- yum update -y \
Note: Run these command in all 4 servers 

Step-4: Jenkins server
- amazon-linux-extras install java-openjdk11 -y
- wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
- rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
- yum install jenkins -y 
- systemctl start jenkins
- systemctl enable jenkins \
Note: Go to AWS console and copy public IP address of jenkins-server and paste in browser with :8080 port. It will ask for Administrator password. Copy path
- cat [paste copied path] \
Note: Password will be displayed on screen, copy and paste in jenkins browser Administrator password field and continue. \
Click on Install suggested plugins \
After installing all plugins, Create First Admin User and go to Jenkins Dashboard
- yum install git -y
- passwd root \
Note: Set your password here
- vi /etc/ssh/sshd_config \
Note: Vi editor page will be open. Find and correct bellow points
- uncomment/enable PermitRootLogin yes
- change PasswordAuthentication 'no' to 'yes' \
Note: Come out from editor using below command
- press ESC button then
- :wq
- systemctl restart sshd
- systemctl status sshd
- ssh-keygen \
Leave blank all
- ssh-copy-id -i root@[ansible-server private IP] \
Note: First time it will ask for password, give password what we had set earlier. And then we can access ansible-server in jenkins-server only. Just to check the same go through bellow command 
- ssh root@[private IP of ansible server]
- exit

Ste-5: ansible server
- amazon-linux-extras install epel -y [epel provide easy access to install commonly used pakages on centos]
- yum install ansible -y
- ansible --version
- passwd root \
Note: Set your password here
- vi /etc/ssh/sshd_config \
Note: Vi editor page will be open. Find and correct bellow points
- uncomment/enable PermitRootLogin yes
- change PasswordAuthentication 'no' to 'yes' \
Note: Come out from editor using below command
- press ESC button then
- :wq
- systemctl restart sshd
- systemctl status sshd
- ssh-keygen \
Leave blank all
- ssh-copy-id -i root@[web-server private IP] \
Note: First time it will ask for password, give password what we had set earlier. And then we can access web-server in ansible-server only. Just to check the same go through bellow command 
- ssh root@[private IP of web-server]
- exit
- vi /etc/ansible/hosts \
Note: Vi editor will be open, mention below line as it is \
[webserver] \
private IP of web-server

Step-6: web-server
- yum install httpd -y
- systemctl start httpd
- systemctl status httpd
- passwd root \
Note: Set your password here
- vi /etc/ssh/sshd_config \
Note: Vi editor page will be open. Find and correct bellow points
- uncomment/enable PermitRootLogin yes
- change PasswordAuthentication 'no' to 'yes' \
Note: Come out from editor using below command
- press ESC button then
- :wq
- systemctl restart sshd

Step-7: dev-server
- yum install git -y
- 





