## Auto Scaling Hands-On


#### 1. Create a Launch Template

Objective :- Define how your EC2 instances should be launched.

Go to EC2 Console → Launch Templates

Click Create launch template

  - Name: web-server-template
  - AMI: Amazon Linux 2
  - Instance type: t2.micro
  - Key pair: Select or create one(testkey.ppk)
  - Security group: Allow SSH (port 22) and HTTP (port 80)
  - User data (optional): Add a startup script to install a web server
    
```
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "Welcome to Auto Scaling Hands-on" > /var/www/html/index.html
```









