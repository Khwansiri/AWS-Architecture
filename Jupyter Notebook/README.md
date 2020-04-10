# AWS: Jupyter Notebook Connection

## Objective 
1. Set Up Jupyter Notebook with Python 3 on Ubuntu 18.04  
2. Create a virtual environment to Python by 2 different ways;   
   - Virtualenv/venv
   - Anaconda  
3. Add virtual environment to Jupyter Notebook by using ipykernel  


#### This project also include the additional information as follows;  
- Create a virtual environment using pew command  
- Install and Uninstall Python package using pip  
- Install and Uninstall Python package using Anaconda

## Set Up Jupyter Notebook with Python 3 on Ubuntu 18.04  
1. Create EC2 instance  
   1.1 Go to AWS Management Console in order to create VPCs, Internet Gateway and Route Table 
       (For more details about how to create and launch EC2 instance, look at repository "AWS-RStudio")  
   1.2 For Security Groups, set inbound rules for SSH, port 22 and TCP, port 8888  
       Note that in this example, I used source from my IP address (Source/ MyIP) in order to have a good habit for more security 
   
   
<img src="https://user-images.githubusercontent.com/57988473/70392713-3f63d800-19e3-11ea-92e2-0fd38b27dbe8.png" width="500">
   
   1.3 Lauch instance  
       For this project, I choose AMI as Ubuntu version 18.04  
       
2. Set up Python in Ubuntu  
   2.1 Update the local package in Ubuntu: 
       ```sudo apt update```    
   2.2 Install pip and the Python header files (Ubuntu 18.04 comes preinstalled with Python 3.6):  
        ```sudo apt install python3-pip python3-dev```  

## Create a Python Virtual Environment for Jupyter (Using Virtualenv/env)  
1. Upgrage pip and install the package:  
   ``` 
   sudo -H pip3 install --upgrade pip  
   sudo -H pip3 install virtualenv  
   ```  
2. Create and move environment into a directory where we can keep our project files  
   ```
   mkdir ~/projectname_dir  
   cd ~/projectname_dir
   ```  
 3. Create a Python virtual environment within this project directory:  
    ``` virtualenv projectname_env```    
 4. Activate the virtual environment:  
    ```source projectname_env/bin/activate```    
    See that the prompt changes to indicate that we are now in a Python virtual environment:                
    ```(projectname_env)user@host:~/project_dir$```    
 5. Install Jupyter:  
    ```pip install jupyter```  
 6. Run Jupyter Notebook:  
    ```jupyter notebook```    
 7. You will see the URL to connect to your Jupyter Notebook as follow;  
 
<img src="https://user-images.githubusercontent.com/57988473/70393169-b307e400-19e7-11ea-968a-27ab88733bb5.png" width="600">
    
#### Note that the printed url indicate "localhost" means that it connect to Jupyter Notebook in your PC (in case you have one) not Jupyter Notebook in Ubuntu server.  
#### Therefore, in case you have Jupyter Notebook in your PC, you have to do some additional step below (Step 8-12)   
  8. Shutdown Jupyter Notebook server (by typing ctrl-C and type "y" to confirm the shutting down step)  
  9. Type the command to allows every IP address for Jupyter Notebook server:  
      ``` jupyter notebook --ip=0.0.0.0 ```  
      
<img src="https://user-images.githubusercontent.com/57988473/70393353-31b15100-19e9-11ea-8a58-02bc8d6d6404.png" width="500">  
      
  10. Copy the new URL to your web browser  
  11. Go to AWS website and copy IPv4 Public IP of your Ubuntu instance  
  12. Replace IP address that is printed in the prompt with Public IP of your instance
    
<img src="https://user-images.githubusercontent.com/57988473/70393106-337a1500-19e7-11ea-94e7-476c358a381a.png" width="800">  

<img src="https://user-images.githubusercontent.com/57988473/70393109-3b39b980-19e7-11ea-8776-baf3446b4d4b.png" width="500"> 

  13. End result of Jupyter Notebook server connection
<img src="https://user-images.githubusercontent.com/57988473/70393112-3f65d700-19e7-11ea-8296-6b902ff7ec4f.png" width="700">  

#### Additional information: Create new environment with terminal in Jupyter Notebook      
1. Select New/ Terminal  
     
 <img src="https://user-images.githubusercontent.com/57988473/70393560-99689b80-19eb-11ea-9771-ce6c7c459d56.png" width="400"> 

2. Create new environment using pew command:    
   ``` 
   pip3 install pew
   pew new environmentname
   ```    
3. Type "y" and press "enter" as required    
   See that we are in the new virtual environment now as indicated as ``` environmentname user@IP:~/projectname_dir$```  
     
<img src="https://user-images.githubusercontent.com/57988473/70393563-9c638c00-19eb-11ea-8afb-b6761435ab1f.png" width="700">      


#### Additional information: Install and uninstall package using pip  
Install package:  
```python -m pip install packagename```  
Uninstall pakage:  
```pip uninstall packagename```  

### Add virtual environment to Jupyter Notebook by using ipykernel  
```python -m ipykernel install --user --name=environmentname```
The new virtual environment will be shown in "New" after you refresh Jupyter Notebook server  
For this example, I add virtual environment name "projectpew" as below  

<img src="https://user-images.githubusercontent.com/57988473/70394015-da16e380-19f0-11ea-9d93-90eeb80b2503.png" width="700"> 
<img src="https://user-images.githubusercontent.com/57988473/70394016-db481080-19f0-11ea-90b7-01ea39b0aa56.png" width="700"> 


## Create a Python Virtual Environment for Jupyter (Using Anaconda)
1. Update new instance  
   ``` sudo apt-get update ```  
2. Find the lates version of Anaconda For Python at Anaconda Download page  
   https://www.anaconda.com/distribution/  or https://repo.anaconda.com/archive/  
   Copy the link address of .sh for the next step
3. Connect to Ubuntu Instance and type the commands belows:  
   ```
   # Change URL as the newest version from step 1
   wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh
   sudo bash Anaconda3-2019.07-Linux-x86_64.sh
   ```
4. Press enter to read the license agreement and type "yes" to accept the license terms   
5. Press enter to install Anaconda to the default location
6. Type yes to have Anaconda update your PATH   
7. The installer will also ask you whether you would like to download and install Visual Studio Code.  
   In my case, I choose "no"  
   
<img src="https://user-images.githubusercontent.com/57988473/70617511-0610af80-1c11-11ea-89a2-eb2aeba35329.png" width="700">  

8. Activate and Update Anaconda installation  
   ``` 
   source ~/.bashrc
   conda update --all --yes
   ```  
9. Start Jupyter Notebook  
   ``` 
   jupyter notebook --ip=0.0.0.0
   ```  
   _Don't forget to change IP address in the printed url to IP address of your instance_  
   
#### Additional Information 
Create new virtual environment  
```
conda create -n environmentname python=3.6
conda activate environmentname 
```  


Remove an environment  
```conda env remove -n environmentname```  
    
#### Additional Information  
Check the packages    
``` conda list ```    
Install package with conda    
``` conda install --name envrinomentname packagename```  
Uninstall package with conda    
``` conda remove --name environmentname packagename```  

## Add virtual environment to Jupyter Notebook by using ipykernel  
```
conda install ipykernel
ipython kernel install --user --name=nameyouwanttodisplay
```



