## Auto Scaling Hands-On


#### 1. Create a Launch Template

Objective :- Define how your EC2 instances should be launched.

Go to EC2 Console → Launch Templates

Click Create launch template

  - Name: myweb-server-template
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

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html>
<head>
    <title>Auto Scaling Hands-on</title>
</head>
<body>
    <h1>Welcome to the Auto Scaling Session</h1>
    <p>This page was deployed using a user data script.</p>
</body>
</html>
EOF

```

  - Click Create launch template


#### 2. Create an Auto Scaling Group (ASG)

Objective :- Automatically manage EC2 instances based on demand.

Go to EC2 Console → Auto Scaling Groups

Click Create Auto Scaling group

  - Name: myweb-asg
  - Select the launch template created above (myweb-server-template)
  - Choose network : Select a VPC and at least 2 subnets in different Availability Zones
  - Configure group size and scaling policies :

      - Desired capacity: 1
      - Minimum capacity: 1
      - Maximum capacity: 4


#### 3. Add Scaling Policies

Objective :- Define when and how your ASG should scale.

Choose Target tracking scaling policy :

  - Metric type : Average CPU Utilization
  - Target value: 50%

Click Next and Create Auto Scaling group

#### 4. Generate Load to Trigger Scaling

Objective :- Simulate traffic to force Auto Scaling to act.

SSH into one of your EC2 instances

Run a stress test :

```
# 1. Install EPEL (Extra Packages for Enterprise Linux)
sudo amazon-linux-extras install epel -y

# 2. Now install stress
sudo yum install stress -y
stress --version
```


This will push CPU usage and should trigger your Auto Scaling policy if it's configured to scale based on CPU.

```
stress --cpu 2 --timeout 300
```

Monitor EC2 → Auto Scaling Groups → Activity to see instances scaling up/down


#### 5. Attach to a Load Balancer (Optional but Recommended)

Objective :- Distribute traffic to your EC2 instances.

Choose Attach to an existing load balancer or Create new

Select Application Load Balancer (ALB)

Define a target group (e.g., myweb-target-group)

Health check path : /



-----------------------------------------------------------------------------------------
















