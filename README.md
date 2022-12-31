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
Note: Run these command in all 4 servers \

Step-4: Jenkins server
- amazon-linux-extras install java-openjdk11 -y
- wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
- rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
- yum install jenkins -y \
- systemctl start jenkins

Ste-5: ansible server
- amazon-linux-extras install epel -y [epel provide easy access to install commonly used pakages on centos]
- yum install ansible -y
- ansible --version

Step-6: web-server
- yum install httpd -y
- systemctl start httpd
- systemctl status httpd
- 





