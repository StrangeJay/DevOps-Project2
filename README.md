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
 
*Refer to [project1](https://github.com/StrangeJay/DevOps_Journey) to see how to connect to your EC2 instance using putty.* 

---
## Step 1 – INSTALLING THE NGINX WEB SERVER
In order to display web pages to our site visitors, we are going to use a high-performance web server called **Nginx**. We’ll use the 'apt' package manager to install this package. 

- Update your server’s package index. Following that, use 'apt install' to get Nginx installed 

  > `sudo apt update` 
  
  > `sudo apt install nginx` 
   
- When prompted, enter **Y** to confirm that you want to install Nginx. Once the installation is finished, the Nginx web server will be active and running on your Ubuntu server. 
   
-  to verify that Nginx is active and running, run the following code: 
 `sudo systemctl status nginx` 

You should get a message that looks like this 
![Screenshot_20221205_133939](https://user-images.githubusercontent.com/105195327/205639669-58c75c4d-84f9-4b17-9ef0-b50f8dc1541f.png)  
 
*If it's green and running, then you have successfully launched your Nginx web server.* 

 
Now it is time for us to test how our Nginx server can respond to requests from the Internet. 
- Open a web browser of your choice and type in your public IP address 

*If you see the following page, then your web server was correctly installed and is accessible.*  
![Screenshot_20221205_134609](https://user-images.githubusercontent.com/105195327/205640977-5c144a94-b6ba-4abe-aeaa-a26ecee00541.png)  
 
  
---
## Step 2 — INSTALLING MYSQL 
Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. *MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.* 

 
- Use the 'apt' package manager to install MySQL 

`sudo apt install mysql-server` 

- When prompted, confirm installation by typing Y, and then ENTER. 
- After installation has been completed, log into the MySQL console by typing 
`sudo mysql` 

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. 
You should see an output like this: 
![Screenshot_20221205_140927](https://user-images.githubusercontent.com/105195327/205645182-fe0d3a52-6929-4aaf-adc1-5fac33ae75ef.png)  
 
  
It’s recommended that you run a security script that comes pre-installed with MySQL. *This script will remove some insecure default settings and lock down access to your database system.*   

Before running the script, you will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as devops111 

> `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'devops111';` 

Exit the MySQL shell with: 
`mysql> exit` 
 
 
