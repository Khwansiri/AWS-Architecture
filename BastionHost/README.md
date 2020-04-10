# AWS: Ping google.com from private subnet through Jumb Box

## Objective:  
Create 2 subnets, private and public, in the same VPC.   
Connect private subnet through Jumb box in public subnet  
then  ping google.com from instance in private subnet through NAT instance instance in public subnet.


<img src="https://user-images.githubusercontent.com/57988473/69799335-5f212200-11d3-11ea-9231-eac4a53baaa9.png" width="500">

## Step Overview:
1. Create VPC
2. Create 2 subnets, private and public  
3. Create Internet Gateways 
4. Create 2 Route Tables  
   * One for Private subnet  
   * One for Public subnet
5. Create 3 Security Groups for each instances
6. Create 3 Instances  
   * Place 2 instances in public subnet. Name them as Jumb Box and NAT Instance. 
   * Place 1 instance in private subnet with the name Final Instance.  
7. Associate Elastic IPs to instances in public subnet    
8. Test connection (Using Putty and Pageant)
   * Connect to Jump Box  
   * From Jump Box connect to Final Instance in private subnet  
   * ping google.com from Final Instance (through NAT Instance)
  
## Step 1: Create VPC  
1. Go to AWS Management Console  
2. Find Services/ VPC  
3. Your VPCs/ Create VPC by using IPv4 CIDR block  (e.g. 10.0.0.0/16 ) 


<img src="https://user-images.githubusercontent.com/57988473/69551539-26980300-0f9d-11ea-8bae-1c2d6405e54f.png" width="500">


## Step 2: Create 2 subnets, private and public  
1. Go to Subnets/ Create subnet
2. Choose VPC that was created from the previous step
3. Set 2 differents IPv4 CIDR block for each subnets  (e.g. 10.0.1.0/24 and 10.0.2.0/24 ) 


<img src="https://user-images.githubusercontent.com/57988473/69552139-395f0780-0f9e-11ea-97fd-7ad31e55afef.png" width="500">
   

## Step 3: Create Internet Gateways
1. Go to Internet Gateways/ Create internet gateway
2. After created, the State of our Internet gateway will be shown as "detached"  


<img src="https://user-images.githubusercontent.com/57988473/69553102-dc645100-0f9f-11ea-93bb-9494f133f102.png" width="500">
 
3. Attach the new Intenet Gateways by choosing that new Intenet Gateway/ Action/ Attach to VPC  


<img src="https://user-images.githubusercontent.com/57988473/69553220-1b92a200-0fa0-11ea-9f5d-67f0357658bc.png" width="500">

4. Choose the VPC that is created from Step 1
5. See that the State of the new Intenet Gateway changes to "attached"

## Step 4: Create Route Tables 
1. Route Tables/ Create route table  
2. Select the route table, and choose tab "Routes"/ Edit routes 
3. See that the first Destination is our VPC and the Target is local.  
   Now we will add route to public by choosing Add route/ set Destination to "0.0.0.0/0"  
   Set Target to new Internet Gateways that is created from Step 3   
   

<img src="https://user-images.githubusercontent.com/57988473/69554184-b17afc80-0fa1-11ea-9732-ba3df3fc954b.png" width="500">


4. Go to Subnet Association tab/ Edit/ Associate/ Choose Public subnet that is created from Step 2  
5. Do the same in order to create a second route table for private subnet.  
   But in Add route process after set Destination to "0.0.0.0/0", choose Target as Instance and choose NAT Instance (Because we will connect to the internet through NAT Instance)  
   Associate Route Tables with private subnet. 
 
 
<img src="https://user-images.githubusercontent.com/57988473/69756282-63f5bf80-115a-11ea-839f-3fef337b1d90.JPG" width="500"> 
    
## Step 5: Create Security Groups
1. Security Groups/ Create security group  
2. By default, new security groups start with only an outbound rule that allows all traffic to leave the instances.   
   We must add rules to enable any inbound traffic or to restrict the outbound traffic.  
3. We will create 3 Security Groups that will associate with specific instances depends on the purpose of each instance.  
  * Security Group of Jump Box   
  
 
 <img src="https://user-images.githubusercontent.com/57988473/69756595-2c3b4780-115b-11ea-98ee-bb1835a5c04b.png" width="700"> 

  
  * Security Group of NAT Instance (Allow all local connection go outside)  
  
  
 <img src="https://user-images.githubusercontent.com/57988473/69756902-dca94b80-115b-11ea-954b-4fb37a577847.png" width="700"> 
 
  
  * Security Group of Final Instance (Allow the access from Jump Box security group only)
  
  
  <img src="https://user-images.githubusercontent.com/57988473/69757166-90124000-115c-11ea-8d40-f697ccf10142.png" width="700"> 
  
    
## Step 6: Create Instances  
   * Jump Box Instance 
     1. Go to AWS Management Console/ Find Services/ EC2    
     2. Instance/ Launch Instance  
     3. Choose AMI as Amazon Linux 2 AMI (HVM)  
     4. Choose Instance Type as General purpose/ t2.micro  
     5. In Configure Instance Details, choose the VPC and Public Subnet that are created from the previous step.  
     6. Do nothing for these 2 upcoming processes; Add Storage and Add Tags  
     7. For the Configure Security Group process, choose "Select an existing security group"    
     8. Apply Security Group of Jump Box as mentioned at Step 5    
     9. Review Instance Launch and download Key pair  
     ### For more security, we should use different Key pair between Jump Box and Other Instances  
     
  * NAT Instance  
    Repeat step 1 and 2  
    Choose community AMIs/ amzn-ami-hvn (Select the most updated version)  
    
    
  <img src="https://user-images.githubusercontent.com/57988473/69757860-57736600-115e-11ea-91c1-4766b8297b35.JPG" width="800"> 
    
    
    Then repeat step 4- 9 as above (For Security Group, choosing Security Group of NAT Instance)  
    After launch, choose NAT Instance/ Actions/ Networking/ Change Source/ Dest Check/ Yes, Disable 
    
   
  <img src="https://user-images.githubusercontent.com/57988473/69758141-fc8e3e80-115e-11ea-8260-4325e4235630.JPG" width="500">
      
    
   * Final Instance  
     Repeat all step as Jump Box but choosing Private Subnet and Security Group for Final Instance   
             
## Step 7: Associate Elastic IPs to instances in public subnet  
1. Elastic IPs/ Allocate new address/ Amazon pool  
2. Choose Elastic IPs/ Actions/ Associate address  
3. Resource type/ Instance  
4. Instance/ Jump Box  
5. Get Private IP/ Associate  
6. Do the same in order to associate with NAT Instance  

## Step 8: Test Connection (Using Putty and Pageant)  
1. Download Putty, PuTTY gen  and Pageant  
   https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html  
2. Go to PuTTY Key, load all existing private key files  
3. Go to Pageant Key List, add all private key files  
4. Go to Jump Box Instance, choose action, copy ec2-user@IP


<img src="https://user-images.githubusercontent.com/57988473/69758668-6ce98f80-1160-11ea-94ad-3478be246eba.JPG" width="500">


5. Go to Putty, pass the information that we copy from the previous step to "Host Name"


<img src="https://user-images.githubusercontent.com/57988473/69758779-bb972980-1160-11ea-8764-2794487e65d4.JPG" width="300">


6. At Category of Putty, go to Connection/ SSH/ Auth 
   Click at "Allow agent forwarding" at Authentication parameters then click "Open"  
7. Now we are in the Linux Instance of Jump Box   
8. Go to Final Instance, copy private IP
9. Go to command line, type ssh then pass the copies private IP address from Final Instance  
10. ping google.com from Final instance  


<img src="https://user-images.githubusercontent.com/57988473/69759448-7247d980-1162-11ea-8a41-e05f80a2be31.JPG" width="500">









