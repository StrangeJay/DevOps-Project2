# DevOps-Project2
## WEB STACK IMPLEMENTATION (LEMP STACK) *Linux, Nginx, MariaDB (MySQL), and PHP* 

## Step 0 – Preparing prerequisites 

In order to complete this project, you will need an AWS account and a virtual server with Ubuntu Server OS. 
If you do not have one, endeavour to create one [here](https://aws.amazon.com/resources/create-account/)

### Creating Our EC2 Instance 
- Search for EC2 on the search bar, or find it in *Services* 
- On the EC2 page, click on launch instance 
![Screenshot_20221205_125856](https://user-images.githubusercontent.com/105195327/205633276-8188beb2-7287-45b5-b3b3-ac55b890b5c7.png)  
 
  
  
- Set the name you want to give your Instance
 ![Screenshot_20221205_130008](https://user-images.githubusercontent.com/105195327/205633794-c98e0a24-106a-48d8-9148-82167631c001.png)  

  
  
- Select the OS to be used for the project, i selected the Ubuntu server, covered by my free tier account
![Screenshot_20221205_130047](https://user-images.githubusercontent.com/105195327/205633826-89c61432-dd1d-44db-aa1d-8f2c86e423a6.png)  
 
  
  
- Choose instance Type and key pair. For Instance type i chose the T2 micro(free tier elligible). And i used a pre-existing keypair i created for the LAMP project. 
![Screenshot_20221205_130129](https://user-images.githubusercontent.com/105195327/205634279-2cfea91d-49f2-40e4-849d-9d2596086470.png)  
 
 
 
- Leave the VPC and Subnet settings on default. Go to the Firewall(Security Group) session. 
- Create a security group, or use an existing Security Group. *For the purpose of this task, i used the SG i created for the LAMP project, 
with inbound rule that allows SSH, HTTP and HTTPS from anywhere. 
![Screenshot_20221205_130216](https://user-images.githubusercontent.com/105195327/205635075-8e6601a8-5389-4cc0-81e5-35bcdd4c541f.png)  
 
  
  
- Click on **Launch Instance** and launch your Instance. 
- Go to the **Instances** page and check if the instance is running, and the status check has the "2/2 checks passed" sign. 

![Screenshot_20221205_130637](https://user-images.githubusercontent.com/105195327/205635666-29f3b3b9-9cbb-47ce-b58e-69af06e64cfc.png)
 
  
   
 - Click on the radio button beside the instance name, a drop down menu would appear. 
 Copy the public IP to a note pad, we will make use of it when connecting to our instance via putty. 
 
 
 
