# AWS: Apply a Convolutional Neural Networks (CNN) for MNIST Handwritten Digit Classification 

## Overview: 
<img src="https://user-images.githubusercontent.com/57988473/71115943-62586e00-21d3-11ea-8655-ba13879307d4.png" width="700">

## Step overview: 
1. Train our model with Keras in Training Machine  
   The model will be created directly and we will use that for further prediction
2. Apply Flask back end to serve our saved model in Back End Server
3. Deploy the code that will read the movement of the mouse to then draw pixels on the screen  
   in order to predict and return the output right to html in Front End Server
   
   *Reference of codes in this repository: Dr. Léo Souquet https://github.com/leodsti/AWS_Tutorials/tree/master/MNIST*  
  
 ## Training Machine  
1. Create an instance as follows;  
    - Ubuntu 18.04 
    - Instance Type: m5ad.xlarge (For the rapidity and efficiency in training process)
    - Public subnet 
    - Security group inboud rule for SSH port 22 and Custom TCP Rule port 8888  
      (for Jupyter Notebook in the later step)  

    *For more details about instance creation, go to https://github.com/Khwansiri/AWS-Architecture/blob/master/Bastion%20Host/README.md *   

2. Connect to instance then lauch Jupyter Notebook by using Anaconda  
   - Create a new virtual environment + install ipykernel for the new environment  
   ```
   sudo apt-get update
   wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh
   sudo bash Anaconda3-2019.07-Linux-x86_64.sh
   source ~/.bashrc
   conda update --all --yes
   # In case you have an error massage "conda command not found" after installation, 
     using this command: export PATH=~/anaconda3/bin:$PATH  
   # or type "exit" and relauch the instance
   jupyter notebook --ip=0.0.0.0
   conda create -n environmentname python=3.6
   conda activate environmentname 
   conda install ipykernel
   ipython kernel install --user --name=nameyouwanttodisplay
   ```
   *For more details about Jupyter Notebook and Anaconda, go to https://github.com/Khwansiri/AWS-JupyterNotebook*   
   
3. Copy files that required for CNN training from this Github repository;  
     ``` 
     # Step1: Go to github repository that have the files needed  
     # Step2: Select Clone or download 
     # Step3: Copy the url then lauch the following commands in terminal of Jupyter Notebook
     
     sudo apt-get install git  
     git clone URLofGithubRepository
     
     ```
 <img src="https://user-images.githubusercontent.com/57988473/71127310-28936180-21eb-11ea-81ed-7050dc9b3528.png" width="800">  

 
 
4. Now we have our repository folder in Jupyter Notebook server 
   - Install and upgraded packages as indicated in requirements_train.txt using command:     
      ```pip install -r requirements_train.txt```  
   - Lauch train.ipynb
   - After training, we will get our model as keras file named "cnn-mnist"


## Back End Server
1. Create an instance as follows;  
    - Ubuntu 18.04 
    - Instance Type: t2.micro
    - Public subnet   
    *Since Back End Server contains sensitive data*      
    *It is recommended to put this server in Private subnet and access through Bastion Host for more security*  
    *More details about Bastion Host in https://github.com/Khwansiri/AWS-BastionHost*  
    - Security group inboud rule for SSH port 22 and Custom TCP Rule port 5000 (For Flask app)
2. Connect to instance then install Anaconda and create new virtual environment
   ```
   sudo apt-get update
   wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh
   sudo bash Anaconda3-2019.07-Linux-x86_64.sh
   source ~/.bashrc
   conda update --all --yes
   conda create -n environmentname python=3.6
   conda activate environmentname 
   ```
3. Copy files from this repository as what we do previously with Training Machine.  
   Files needed for Back End server are our model from Training Machine: cnn-mnist, app.py and requirements_app.txt 
   Therefore, we will copy these 3 files to our instance 
   ```
   sudo apt-get install git
   git clone URLofGithubRepository
   ls
   # Go to directory of the file that you want to copy with the command cd
   # In this case, my files are in AWS-MNIST folder
   cd AWS-MNIST
   ls
   # ls to see the name of all file in the directory, so that we will make less mistake when typing
   # You will see cnn-mnist, app.py and requirements_app.txt after ls AWS-MNIST 
   # Now we will copy one by one to our instance 
   # The command structure is: sudo cp -r FileThatYouWantToCopy DestinationDirectory
   sudo cp -r cnn-mnist /home/ubuntu/ 
   sudo cp -r app.py /home/ubuntu/
   sudo cp -r requirements_app.txt /home/ubuntu/
   cd 
   ls
   # This ls is to make sure that you copy all files needed
   ```
4. Lauch Back End Server  
   ``` 
   conda create -n backend
   conda activate backend
   conda install opencv
   pip install -r requirements_app.txt
   python app.py
   ```
5. Now we will prepare file for Front End Server  
   - In this repository, you will see file "index.html" 
   - Copy Public IP address of your Back End Server and paste it in indext.html file as below
   
 <img src="https://user-images.githubusercontent.com/57988473/71127309-28936180-21eb-11ea-9c30-d9ecdd27e7b5.png" width="800">

## Front End Server 
1. Create an instance as follows;  
    - Ubuntu 18.04 
    - Instance Type: t2.micro
    - Public subnet 
    - Security group inboud rule for SSH port 22 and HTTP port 80 (For web access)
2. Connect to instance  
   We will use Apache as our html presentation layer   
   ``` 
   sudo apt-get update
   sudo apt-get install apache2
   ```
   Then copy Public IP address of our instance to web browser, you should see something like this  
<img src="https://user-images.githubusercontent.com/57988473/71124843-064b1500-21e6-11ea-8351-d4cd5c505241.png" width="500">

3. Copy files from this Github repository  
   For Front End Server, we need index.html and static folder  
   ``` 
    sudo apt-get install git  
    git clone URLofGithubRepository
    # Our destination directory is /var/www/html/
    # To get the file that we want to copy, in case you don't want to go inside directory,
    # you can copy file this way
    sudo cp -r AWS-MNIST/index.html /var/www/html/
    sudo cp -r AWS-MNIST/static /var/www/html/
    # Recheck the copied files
    cd /var/www/html/
    ls
    ```
4. After copy all files needed, when you refresh the Apache browser  
   You should see you app like this 
<img src="https://user-images.githubusercontent.com/57988473/71124844-06e3ab80-21e6-11ea-8f2c-a5154ee31172.png" width="500">    

5. Now it's time to be pround and enjoy your first Machine Learning Project!!
