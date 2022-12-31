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

Step-8: Create one repo, under that create a file,  name it as index.html and simply mention 'Hi' in this page \
Step-9: Go to jenkins web page Dashboard \
Step-10: Create one job/item, give name as 'demo-project' choose freestyle project \
Step-11: Dashboard -> demo-project -> Source Code Management -> choose Git and paste URL of github Repo -> Apply and Save \
Step-12: Dashboard -> Manage Jenkins -> Plugin Manager -> Available plugins -> Search for SSH -> Choose 'Public Over SSH' -> Install and restart \
Step-13: Dashboard -> Manage Jenkins -> Configure System -> Scroll Below, we can find SSH Servers -> Click on Add \
Name - Jenkins \
Hostname - private IP of Jenkins \
Username - root \
Click on advance - Tick on Use password authentication \
Password - Give password which we had set in jenkins server \
Click on 'Test Configuration', If correct it will display 'success' \
Click on apply \
Click on add \
Name - Ansible
Hostname - private IP of ansible-server
username - root
Click on Advance -  Tick on Use password authentication \
Password - Give password which we had set in ansible server \
Click on 'Test Configuration', If correct it will display 'success' \
Click on apply  & Save \
Step-14: Dashboard -> demo-project -> Configure -> Scroll Down to Build Steps  \
Click on Add build step -> choose 'Send files or execute commands over SSH' \
Under 'Name' two options available there Ansible & Jenkins \
Select Jenkins \
Under 'Exec Command' mention below line as it is \
rsync -avh /var/lib/jenkins/workspace/demo-project/*.html root@privateIPofAnsible:/opt/index.html \
Click on Apply \
Under 'Post-build Actions' \
Choose 'Send build artifacts over SSH' \
Under name select 'Jenkins' \
Under 'Exec Command' mention below line as it is \
ansible-playbook /sourcecode/deployment.yml \
Apply & Save \ 

Step-15: Go to ansible server 
- mkdir /sourcecode
- cd /sourcecode
- vi deployment.yml \
Note: Vi editor will be open, mention below line as it is
- name: Pic file from source and push into destination \
  hosts: webserver \
  tasks: \
    - copy: \
        src: /opt/index.html \
        dest: /var/www/html \
Save and exit from vi editor using below command \
Click 'ESC' button and then
-:wq \
Step-16: Go to web-server
- cd /var/www/html
- pwd
- ls \
Note: No file and folder \ 

Step-17: Go to github -> first-demo repo \
Settings -> Webhooks -> Add Webhook \
In the field of 'payload URL' - paste jenkins web URL/github-webhook/ \
In 'content type' - choose 'application/json' \
Step-18: For secret field, go to Jenkins Dashboard \
Under 'User' - choose Configure - Scroll to 'API Token'  \
Add new Token -> Generate \
Copy Token and paste it to Github 'secret' field  \
Click on 'Add webhook' 

Step-19: Jenkins dashboard -> demo-project -> Configure -> Build Triggers - Chose Github hook trigger for GITScm polling \
Apply & Save

Congratulation All done

Just copy web-server public IP and paste in browser

You will get 'Hi' which we had written in html file in github

Just do some changes in your html code and directly see changes in browser, Jenkins will do all CICD and deploy it to browser

We have created CICD successfully using Git, Github, Jenkins, Ansible and many more

Step-20: Go to dev-server
- git clone [repository URL]
- ls
- cd first-demo
- ls
- vi index.html \
Note: Vi editor will be open, write below html code

![image](https://user-images.githubusercontent.com/102685509/210155334-908ff521-2bfa-422d-8445-d28b16434b5c.png)

Save and exit from vi editor
- git add .
- git commit -m "html code updated" index.html
- git config --global user.email "codingmywork@gmail.com"
- git config --global user.name "MSFaizi"
- git commit -m "html code updated" index.html
- git push -u -f origin main 
- usernamer[put github username and password]









