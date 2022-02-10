# Create a EC2 Instance and deploy a django in it 
## Step 1 : Create a EC2 Instance in AWS
   1. Go to AWS EC2 Console and click on Launch Instance
   2. Select the desired AMI for now select Ubuntu image under free Tier
   3. Choose the instance type as per the requirements
   4. On configure Instance details select the Desired configuration `Specially Network selection ,VPC,and IAM Role as per the required need`
   5. Select the Desired Security Group `(Specially Allow network Traffic Form HTTP,TCP and from other protocol)`
   6. See all the configuration and click on launch
   7. Now a Window will popup asking to selecta key pair this key pair are used to access your EC2 Machine From your dekstop after that launch the instance .


![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/New%20folder/EC2AMI.png)


![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/New%20folder/EC2Securitygrp.png)

## Step 2 : Connect with your Ec2 Machine
  1. There are mainly 2 ways one by Ec2 Instance Connect which will open the terminal of your EC2 Virtual Machine inside your Browser or by SSH Client by using key pair that you created 
  2. Connect with any of the way 

## Step 3: deploy your App on this EC2 Virtual Machine
   1. Once you have connected with your machine inside the terminal you can check with basic commands such as PWD ,ls that will run on your Virtual Machine that aws has created for you somewhere .
   2. To deploy any app here you need to copy your code base to this Virtual Machine Either clone your github repo or by your local machine to this virtual machine
   3. for now will we will clone a Django app from github to this Virtual machine
   Know here are some basic commands that you need to run
   ```python
sudo apt update & sudo apt upgrade    
sudo apt install python3-pip
python3 -m pip install --upgrade pip
pip install django==3.2 

python3 --version
pip --version
django-admin --version
```

After cloning  your app to this machine run the command Python manage.py runserver from the desired directory

Finally, visit public_ip:8000 in your web browser to access your project.

![alt text](https://github.com/manmohan1105/AWS-Tasks-asssigned-by-Mentor-/blob/main/New%20folder/Instance%20summary.png)
