# AWS: Connect to an RDS MySQL DB instance using Python in EC2 instance  

## Objective:  
Create 3 subnets, 1 public and 2 privates, in the same  VPC.  
The web server EC2 instance is hosted in the public subnet, so that it can reach the public Internet.   
The RDS database (DB) instance is hosted in a private subnet.  
The EC2 instance is able to connect to the DB instance because it is hosted within the same VPC,  
but the DB instance is not available to the public Internet, providing greater security for our database. 


<img src = "https://user-images.githubusercontent.com/57988473/70390329-4b8f6b80-19ca-11ea-829c-a6de965dc663.png" width="500">



## Step Overview:  
1. Create VPC  
2. Create Internet Gateway  
3. Create Public Subnet (+ Route Table + Security Group) 
4. Create 2 Private Subnets in different availability zone  (+ Route Table + Security group) 
5. Create EC2 Instance in Public Subnet
6. Create RDS database (DB) Instance in one of Private Subnet
7. Connect DB instance through EC2 Instance  



## Step 1: Create VPC    
1. Go to AWS Management Console    
2. Find Services/ VPC   
3. Your VPCs/ Create VPC by using IPv4 CIDR block  (e.g. 10.0.0.0/16 )  



<img src = "https://user-images.githubusercontent.com/57988473/70377325-9bf7c200-1913-11ea-9320-18ba3161e748.png" width="600">



4. Enable DNS resolution by   
   Choose VPC that we want/ Actions/ Edit DNS resolution/ enable/ Save


<img src = "https://user-images.githubusercontent.com/57988473/70377328-a1550c80-1913-11ea-882c-c45361944523.png" width="300">



5. 4. Enable DNS hostname by   
   Choose VPC that we want/ Actions/ Edit DNS resolution/ enable/ Save


<img src = "https://user-images.githubusercontent.com/57988473/70377332-a74aed80-1913-11ea-870d-11567fb80963.png" width="300">



## Step 2:  Create Internet Gateway
1. Go to Internet Gateways/ Create internet gateway  
2. Attach the new Intenet Gateways by choosing that new Intenet Gateway/ Action/ Attach to VPC/ Choose the VPC that is created from Step 1  



<img src = "https://user-images.githubusercontent.com/57988473/70377454-e594dc80-1914-11ea-9424-ae61ec6bb5c2.png" width="600">


## Step 3:  Create Public Subnet (+ Route Table + Security Group)
1. Go to Subnets/ Create subnet/ Set IPv4 CIDR block (e.g. 10.0.1.0/24)
2. Choose VPC that was created from the previous step  

<img src ="https://user-images.githubusercontent.com/57988473/70377459-eb8abd80-1914-11ea-99e5-9f80ae4c70b8.png" width="600">  

3. Create Route Table for this public subnet by going to Route Tables/ Create route table    
   Select the route table, and choose tab "Routes"/ Edit routes   
   See that the first Destination is our VPC and the Target is local.  
   Now we will add route to public by choosing Add route/ set Destination to 0.0.0.0/0  
   Set Target to new Internet Gateways that is created from Step 2  
   
   
 <img src ="https://user-images.githubusercontent.com/57988473/70377460-efb6db00-1914-11ea-9b71-0c5c615a9119.png" width="600">  
   
4. Go to Subnet Association tab/ Edit/ Associate/ Choose Public subnet that is created from 1.


<img src ="https://user-images.githubusercontent.com/57988473/70377462-f2b1cb80-1914-11ea-8c49-9d2486464755.png" width="600">  

5. Create Security Group by go to  Security Groups/ Create security group  
   Add Inbound Rules HTTP from anywhere (0.0.0.0/0, ::/0) and SSH from our IP address only (Source/ MyIP) for more security.


<img src ="https://user-images.githubusercontent.com/57988473/70377463-f5142580-1914-11ea-9586-17dc6147adc9.png" width="500">  




## Step 4:  Create 2 Private Subnets in different availability zone  (+ Route Table + Security group) 
1. Go to Subnets/ Create subnet
2. Choose VPC that was created from the previous step
3. Set 2 differents IPv4 CIDR block (e.g. 10.0.2.0/24 and 10.0.3.0/24 ) 
   and 2 different availability zone for each subnets  (e.g. us-east-1a and us-east-1b)


<img src ="https://user-images.githubusercontent.com/57988473/70377750-7ae5a000-1918-11ea-8cfe-3d165b22cb9a.png" width="500"> 


<img src ="https://user-images.githubusercontent.com/57988473/70377752-7de09080-1918-11ea-9777-58cc5bc52345.png" width="500"> 



4. Create Route Table by to to Route Tables/ Create route table    
   Go to Subnet Association tab/ Edit/ Associate/ Choose Public subnet that is created from 3.
   
   
 <img src ="https://user-images.githubusercontent.com/57988473/70377755-8042ea80-1918-11ea-866e-b39813bf1019.png" width="600">  
    
    
 <img src ="https://user-images.githubusercontent.com/57988473/70377757-82a54480-1918-11ea-822a-8d26e49b09ac.png" width="600">  
 

5. Create Security Group by going to  Security Groups/ Create security group  
   Add Inbound Rules MYSQL/Aurora with Protocol TCP, Port 3306  
   Source = ID of Security Group of Public SUbnet from step 3.5

 <img src ="https://user-images.githubusercontent.com/57988473/70377758-85079e80-1918-11ea-92bd-96077e3f2a0b.png" width="600">  



## Step 5: Create EC2 Instance in Public Subnet
1. Go to AWS Management Console/ Find Services/ EC2  
2. Instance/ Launch Instance  
3. For this project, I choose AMI as Ubuntu Server 18.04 and Instance Type as General purpose/ t2.micro  
4. In Configure Instance Details, choose the VPC and Public Subnet that are created from the previous step.  
5. Do nothing for these 2 upcoming processes; Add Storage and Add Tags  
6. For the Configure Security Group process, choose "Select an existing security group"    
7. Apply Security Group of Public Subnet as mentioned at Step 3    
8. Review Instance Launch and download Key pair  
9. Associate Elastic IPs by go to Elastic IPs/ Allocate new address/ Amazon pool  
10. Choose Elastic IPs/ Actions/ Associate address  
11. Resource type/ Instance/ Select our instance
12. Choose Public IP/ Associate  




## Step 6: Create RDS database (DB) Instance  in one of Private Subnet
1.	Create a DB subnet group  
   - Open the Amazon RDS console/ RDS
   - In the navigation pane, choose Subnet group/ Create DB Subnet Group.
   - On the Create DB subnet group page, set Name, Description and choose our VPC from previous step
   - In the Add subnets section, choose Add all the subnets related to this VPC  
   - Remove Publice Subnet in our VPC
   - Choose Create

 <img src ="https://user-images.githubusercontent.com/57988473/70378120-24c72b80-191d-11ea-891f-00a35fcaf4cf.png" width="600">  

2. Create database as follow;  
   - Choose a database creation method/ Standard Create  
   - Engine options/ MySQL  
   - Templates/ Free tier  
   - Settings/ Add name of DB instance identifier, Master username and password  
   - Storage/ Disable "storage autoscaling"  
   - Connectivity  
      - Virtual Private Cloud (VPC)/ Choose our VPC
      - Additional connectivity configuration/ Choose our Subnet group from previous step    
      - Publicly accessible/ No (For more security, we want to keep our DB private and can be accessed through our EC2 instance only)  
      - VPC security group/ Choose existing/ Choose our Security Group  
   - Additional Configuration/ Name our database in "initial database name"  
     

<img src ="https://user-images.githubusercontent.com/57988473/70378075-9eaae500-191c-11ea-8c6e-12cf1a26c134.png" width="500">   
   
3. End result of DB instance

<img src ="https://user-images.githubusercontent.com/57988473/70378077-a1a5d580-191c-11ea-848e-a33e2904384e.png" width="500">   





## Step 7: Connect DB instance through EC2 Instance  
1. Go to PuTTY Key, load private key files and change into ppk file 
2. Connect EC2 Instance by using Putty
   - Go to EC2 Instance that is created in previous step  
   - Choose action, copy ec2-user@IP  
   - Go to Putty, pass the information that we copy from the previous step to "Host Name"  
   - At Category of Putty, go to Connection/ SSH/ Auth / Select key in ppk format from previous step    
   

<img src ="https://user-images.githubusercontent.com/57988473/70378479-0bc07980-1921-11ea-8118-a535822cb31d.png" width="500"> 

3. We are now in EC2 Linux Instance. 
   Next step is trying to connect to our DB instance through Python.  
   Update and download python package as follow; 
   - Update the package list by using the command: sudo apt update  
   - Install pip for Python3: sudo apt install python3-pip  
   - Install pymysql for Python3: sudo apt-get install python3-pymysql  
   - Run python by typing python3 in command  
4. Connect to DB through python3 with this command;   
   ```
   import pymysql  
   database_instance_endpoint="Endpoint from Our Database"  
   port=3306  
   dbname="database name"  
   user="master user name"  
   password="password of master user name"  
   connection = pymysql.connect("Endpoint from Our Database",    
                      user = "master user name",    
                      port = 3306,    
                      passwd = "password of master user name",    
                      database = "database name")  
   ```
    
   <img src ="https://user-images.githubusercontent.com/57988473/70379047-5d203700-1928-11ea-8cc8-c887ebe25f21.png" width="700"> 

5. We will try to create some data in our DB instance as follow  
   ``` 
   #Create a cursor to query our DB by .cursor()
   mycur = connection.cursor()
   
   #Create a table name "students" that have the information of "name" and "major" both information in string data type
   mycur.execute("CREATE TABLE students (name VARCHAR(255), major VARCHAR(255))")
   
   #Inserting data into table by using INSERT INTO table_name (column_names) VALUES (data type) statement.
   query = "INSERT INTO students (name, major) VALUES (%s, %s)"  
   values = ("John", "Data Science")  
   
   # Execute SQL command by .execute()  
   mycur.execute(query, values)  
   
   #In case you want to add multiple rows at the same time, use the .executemany()
   
   #Getting all records from table, .fetchall()
   
   #Commit the change that we made by .commit()
   
   ```


  <img src ="https://user-images.githubusercontent.com/57988473/70378993-b76cc800-1927-11ea-90db-fd1f762f4d97.png" width="700"> 

            
                      



















