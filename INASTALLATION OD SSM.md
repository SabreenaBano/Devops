## TASK:Install SSM agent on ubuntu server
     make sure you are using session manager
     1.sudo apt update
     2.sudo snap install amazon-ssm-agent --classic
     3.sudo systemctl start amazon-ssm-agent
     4.sudo systemctl enable amazon-ssm-agent
# Jenkins Private setup :
#Create the server in private subnet
Install jenkins on the server
Below is the two way to view the jenkins website
# VPN:
  Create the server in public subnet
  install the pritunl on the server
  configure the pritunl VPN
  Create user in VPN and connect the pritunl VPN using pritunl client https://client.pritunl.com/#install (download the application on your local)
  Hit the jenkins server private in browser
# ELB:
  Create application load balancer
  Create target group and attach the jenkins server
  Hit the load balancer DNS name in browser, get the jenkins dashboard"

# Step 1: Install SSM Agent on Ubuntu Server
AWS Systems Manager (SSM) allows you to manage EC2 instances remotely without using SSH. The SSM agent is pre-installed on some instances, but you may need to install it manually on others.
#Install SSM Agent:
Connect to your Ubuntu EC2 instance.
1.Install the SSM agent using the following commands:
    ** sudo apt update
       sudo snap install amazon-ssm-agent --classic**
2.Start and Enable SSM Agent:
Make sure the SSM agent is running and enabled on boot:
     ** sudo systemctl start snap.amazon-ssm-agent.amazon-ssm-agent.service
        sudo systemctl enable snap.amazon-ssm-agent.amazon-ssm-agent.service **
3.Verify SSM Agent:
 Check the status to ensure the SSM agent is running:
    sudo systemctl status snap.amazon-ssm-agent.amazon-ssm-agent.service
4.SSM IAM Role:
Make sure your EC2 instance is associated with an IAM role that has the AmazonSSMManagedInstanceCore policy to allow SSM communication.
## Jenkins Private Setup (Server in Private Subnet)
 Create EC2 Instance in Private Subnet:
   Launch an EC2 instance in your VPC's private subnet.
   Make sure the instance has no public IP and that the subnet's route table only routes traffic through a NAT Gateway for internet access.
   Ensure the security group allows inbound traffic on port 8080 for Jenkins (only from specific trusted IPs or VPN).
# Install Jenkins on the Server:
Connect to the EC2 instance using SSM (Session Manager).
Install Jenkins by running the following commands:
    **sudo apt-get update
      sudo apt-get install openjdk-11-jdk -y**
     sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
      https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
    echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
          https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
               /etc/apt/sources.list.d/jenkins.list > /dev/null
      sudo apt-get update
      sudo apt-get install jenkins
      sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
      https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
      echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
          https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
          /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt-get update
  sudo apt-get install jenkins
  sudo systemctl enable jenkins
  sudo systemctl start jenkins
  sudo systemctl status jenkins
# Access Jenkins Dashboard:
Jenkins runs on port 8080 by default. Since this server is in a private subnet, you need to access it either via VPN or through an Application Load Balancer (ALB).
# Access Jenkins - Using Application Load Balancer (ALB)
Create an Application Load Balancer (ALB):
In AWS Management Console, go to EC2 > Load Balancers.
Create an Application Load Balancer with the following settings:
Scheme: Internet-facing.
Listener: HTTP on port 80.
VPC: Select the VPC where your private EC2 instance is located.
Subnets: Choose public subnets for ALB.
Create Target Group:
Create a Target Group and attach your private Jenkins EC2 instance to it.
Ensure the target group uses HTTP protocol on port 8080 (the port Jenkins is running on).
The health check can be configured to hit /login or / to ensure Jenkins is running.
Configure Security Groups:
Ensure the security group of your Jenkins instance allows incoming traffic on port 8080 from the ALB security group.
Access Jenkins:
  Once the ALB is set up and connected to the Jenkins instance, you can access Jenkins by visiting the DNS name of the ALB in your browser:
               http://<ALB-DNS-Name>:80
jenkins url : http://jenkins-alb-1834194227.ap-south-1.elb.amazonaws.com/

    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    
    




   
 

            
 