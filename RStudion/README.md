# Connect to RStudio Server by Using AWS Cloud  

## Step Overview:  
1. Create CentOS 7 AWS instance  
   1.1 Create VPCs  
   1.2 Create Subnet  
   1.3 Create Internet Gateway  
   1.4 Create Route Table 
   1.5 Create Security Group  
   1.6 Create Instance
   1.7 Associate Elastic IPs to instance  
2. Connect to instance using command prompt

## Step 1: Create AWS CentOS 7 instance 
### 1.1 Create VPCs 
1. Go to AWS Management Console  
2. Find Services/ VPC  
3. Your VPCs/ Create VPC by using IPv4 CIDR block  (e.g. 10.0.0.0/16 )  


<img src="https://user-images.githubusercontent.com/57988473/69551539-26980300-0f9d-11ea-8bae-1c2d6405e54f.png" width="500">


### 1.2 Create subnet  
1. Go to Subnets/ Create subnet
2. Choose VPC that was created from the previous step
3. Set IPv4 CIDR block (e.g. 10.0.1.0/24)  


<img src="https://user-images.githubusercontent.com/57988473/69552139-395f0780-0f9e-11ea-97fd-7ad31e55afef.png" width="500">
 

### 1.3 Create Internet Gateway
1. Go to Internet Gateways/ Create internet gateway
2. After created, the State of our Internet gateway will be shown as "detached"  


<img src="https://user-images.githubusercontent.com/57988473/69553102-dc645100-0f9f-11ea-93bb-9494f133f102.png" width="500">
 
3. Attach the new Intenet Gateways by choosing that new Intenet Gateway/ Action/ Attach to VPC 

<img src="https://user-images.githubusercontent.com/57988473/69553220-1b92a200-0fa0-11ea-9f5d-67f0357658bc.png" width="500">
  
4. Choose the VPC that is created from Step 1
5. See that the State of the new Intenet Gateway changes to "attached"  
    

### 1.4: Create Route Table
1. Route Tables/ Create route table  
2. Select the route table, and choose tab "Routes"/ Edit routes 
3. See that the first Destination is our VPC and the Target is local.  
   Now we will add route to public by choosing Add route/ set Destination to "0.0.0.0/0"  
   Set Target to new Internet Gateways that is created from Step 3  
   
   
<img src="https://user-images.githubusercontent.com/57988473/69554184-b17afc80-0fa1-11ea-9732-ba3df3fc954b.png" width="500">  
  
4. Go to Subnet Association tab/ Edit/ Associate/ Choose subnet that is created from Step 2    
   
### 1.5: Create Security Groups
1. Security Groups/ Create security group  
2. By default, new security groups start with only an outbound rule that allows all traffic to leave the instances.   
   We must add rules to enable any inbound traffic.  
3. Set inbound rule/ Type: SSH/ Protocol: TCP/ Port Range: 22/ Source: 0.0.0.0/0.  

### 1.6: Create Instances  
1. Go to AWS Management Console/ Find Services/ EC2    
2. Instance/ Laucnch Instance  
3. Choose AMI as Amazon Linux 2 AMI (HVM)  
4. Choose Instance Type as General purpose/ t2.micro  
5. In Configure Instance Details, choose the VPC and subnet that are created from the previous step.  
6. Do nothing for these 2 upcoming process; Add Storage and Add Tags  
7. For the Configure Security Group process, choose "Select an existing security group"    
8. Apply specific Security Group as mentioned at Step 5    
9. Review Instance Launch and download Key pair  

### 1.7: Associate Elastic IPs to instances 
1. Elastic IPs/ Allocate new address/ Amazon pool  
2. Choose Elastic IPs/ Actions/ Associate address  
3. Resource type/ Instance/ Choose instance that is created from the previous step
4. Get Private IP/ Associate 

## Step 2: Connect to instance using command prompt
1. Open command prompt  
2. Go to the directory that have Key pair (from step 1.6)  
3. Type: ssh -i "Name of Key pair" ec2-user@PublicIPofInstance 


<img src="https://user-images.githubusercontent.com/57988473/69565374-21df4900-0fb5-11ea-97b4-4384b74c0f77.png" width="500">  



## Step 3: Execute commands to install RStudio 
1. sudo yum install epel-release  
2. sudo yum update  
3. sudo shutdown -r now  
4. After the reboot, use the same sudo user to log in back.  
5. Install R by execute: sudo yum install R -y  
6. Use the following commands to install the latest stable release of RStudio Server.  
   At November 2019 where this article was written, it is 1.0.136.  
   wget https://download2.rstudio.org/rstudio-server-rhel-1.0.136-x86_64.rpm
   sudo yum install --nogpgcheck rstudio-server-rhel-1.0.136-x86_64.rpm -y  
   *Find the latest release of RStudio Server at https://rstudio.com/products/rstudio/download-server/*  
 7. After the installation, the RStudio Server service should have gotten started.  
    We can check its status and set it to run on boot as below:  
    sudo systemctl status rstudio-server.service  
    sudo systemctl enable rstudio-server.service  
 8. Access RStudio Server from a web browser by modify security group of our instance by adding port 8787. 
 9. Create user name and password for RStudio as follow:  
    sudo useradd YourUsername 
    sudo passwd YourPassword
 10. Go to web browser and type: http://PublicIPofInstance:8787  
11. Connect to RStudio by using user name and password that are set from previous step

## Short cut: 
We can connect to RStudio Server directly when we create our AWS instance  
by putting all command above to Advance Details during Step 3: Configure Instance Details as below:


<img src="https://user-images.githubusercontent.com/57988473/69566538-9adfa000-0fb7-11ea-9309-0683b579bfaf.png" width="500">  

## Note that we have to start the script with: #!/bin/bash  

<img src="https://user-images.githubusercontent.com/57988473/69638144-712b8500-105a-11ea-8a65-d5fa3c0d783d.JPG" width="500">  




