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
 
- Start the interactive script by running: `sudo mysql_secure_installation`  
- You would receive a message asking you to validate password component  
*You can choose to validate or not, depending on how secure you want your password to be. It checks the strength of a password, and allows users to set only passwords that are strong enough.*  
**NOTE:** If enabled, passwords which don’t match the specified criteria will be rejected by MySQL with an error. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials.
![Screenshot_20221206_202812](https://user-images.githubusercontent.com/105195327/206004487-98671b3f-496d-4369-b788-15f392566ad4.png)  
   

- When you’re finished, test if you’re able to log in to the MySQL console by typing: `sudo mysql -p`  
![Screenshot_20221206_203214](https://user-images.githubusercontent.com/105195327/206005111-15f8d025-c569-4456-b0b2-36cd285f2ee3.png)  
  
- To exit the MySQL console, type:
`mysql> exit`  


*Note:* At the time of this writing, the native MySQL PHP library mysqlnd doesn’t support **caching_sha2_authentication**, *the default authentication method for MySQL 8*. For that reason, when creating database users for PHP applications on MySQL 8, you’ll need to make sure they’re configured to use mysql_native_password instead. We’ll demonstrate how to do that in Step 6.


Your Mysql server is now installed and secure, next we would install the final component of the LEMP stack. *PHP*.

---
---
## Step 3 —  INSTALLING PHP  
Unlike Apache that embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. *This allows for a better overall performance in most PHP-based websites*, but it requires additional configuration. 
You’ll need to install `php-fpm,` which stands for **“PHP fastCGI process manager”**, and tell Nginx to pass PHP requests to this software for processing.  
Additionally, you’ll need `php-mysql`, a PHP module that allows PHP to communicate with MySQL-based databases.  

To install these 2 packages at once, run:
> `sudo apt install php-fpm php-mysql`  

When prompted, type **Y** and press ENTER to confirm installation.  
![Screenshot_20221206_205149](https://user-images.githubusercontent.com/105195327/206009077-15f8e513-1e06-48e0-81aa-d82d4f22cdfa.png)  


You now have your PHP components installed. Next, you will configure Nginx to use them.

---
---

## Step 4 — CONFIGURING NGINX TO USE PHP PROCESSOR  
   
With Nginx web server, we can host multiple domains on a single server. By creating server blocks(*similar to virtual hosts in Apache), to encase configuration details.  
In this guide, we will use **projectlemp** as a domain name. 

On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at /var/www/html. This works well for hosting a single site, but it would become a problem when you intend to host multiple sites. Instead of modifying /var/www/html, we’ll create a directory structure within /var/www for your domain website, leaving /var/www/html as the default directory to be served if a client request does not match any other sites. 

- Create the root web directory for your domain as follows: 
> `sudo mkdir /var/www/projectlemp`  

- Assign ownership of the directory with the **$USER** environment variable, which will reference your current system user:  
> `sudo chown -R $USER:$USER /var/www/projectlemp`  

- Open a new configuration file on Nginx's *sites-available* directory using your preferred text editor. I would be using nano: 
> `sudo nano /etc/nginx/sites-available/projectlemp`  

This will create a blank line, post the following bare-bones configuration:  

> #/etc/nginx/sites-available/projectlemp  
> 
> server {
>         listen 80;
>         server_name projectlemp www.projectlemp;  
>         root /var/www/projectlemp;
>
>         index index.html index.htm index.php;
> 
>         location / {
>         try_files $uri $uri/ =404;
>         }
> 
>         location ~ \.php$ {
>         include snippets/fastcgi-php.conf;
>         fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
>         }
> 
>         location ~ /\.ht {
>         deny all;
>         }
>  
> }    
  

![Screenshot_20221207_115307](https://user-images.githubusercontent.com/105195327/206160552-54162777-a8d1-4f2a-bca7-32c509bc28a6.png)  
  
  
Here's what each of the directives and location blocks do:  
- **listen:** This defines the port Nginx will listen on. *In this case it will listen on port 80, the default port for HTTP.*  
- root: This defines the root of the document, where the files served by this server are stored.  
- **index:** This defines the order in which Nginx will prioritize index files for this website.  
- **server_name:** Defines which domain names and/or IP addresses this server block should respond for. *Point this directive to your server’s domain name or public IP address.*

- **location /:** The first location block includes a try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.  
- **location ~ \.php$:** This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.  
- **location ~ /\.ht:** The last location block deals with .htaccess files, *which Nginx does not process.* By adding the `deny all` directive, if any .htaccess files happen to find their way into the document root, they will not be served to visitors.  
  
  
When you're done editing the document, save and close the nano file by typing  
`CTRL + X` and then `Y` and `ENTER` to confirm.  

- Activate your configuration by linking to the config file from Nginx's "sites-enabled" directory: 
> `sudo ln -s /etc/nginx/sites-available/projectlemp /etc/nginx/sites-enabled/`  
 
This would tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing: 
`sudo nginx -t`  

You should see the following mesage:  
![Screenshot_20221207_122033](https://user-images.githubusercontent.com/105195327/206166310-86c5d7f5-a46d-44c8-9623-e8cb2639a98c.png)  
  
- If any errors are reported, return to the configuration page to review its contents before continuing. 

- Disable the default Nginx host that is currently configured to listen on port 80, by typing the following command: 
`sudo unlink /etc/nginx/sites-enabled/default`  
  
- When you are done, reload Nginx to apply the changes:
`sudo systemctl reload nginx`  
  
Congratulations, your new server is active , but the web root `/var/www/projectlemp` is still empty. *Create an index.html file in that location, to test that the new server block works as expected:  

> sudo echo 'Hello lemp from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' 
> $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlemp/index.html  
  
- Go to your browser and open your website using your public IP address:  
  ![Screenshot_20221207_124201](https://user-images.githubusercontent.com/105195327/206170755-a2faa924-1b70-4e4d-97dd-d63e709598a5.png)  
  
  
*If you see the text from the **"echo"** command you wrote to the index.html file, then it means your Nginx site is working as expected.* 

You can leave this file in place as a temporary landing page for your application, until you set up an `index.php` file to replace it. Once you do that, remember to remove or rename the `index.html` file from your document root, as it would take precedence over an index.php file by default. 

---
---
## Step 5 – TESTING PHP WITH NGINX  
At this stage, your LEMP stack is completely installed and fully operational. *You can test it to ensure that Nginx can accurately hand .php files off to your processor.*  
This can be done by ccreating a test PHP file in your document root. 
- Open a new file called **info.php** within your document root in your text editor:  
> `sudo nano /var/www/projectlemp/info.php`  
- Type the following lines into the new file: 
> `<?php 
  phpinfo();`  

  ![Screenshot_20221207_130847](https://user-images.githubusercontent.com/105195327/206175837-89aa4e85-b3d9-4cc4-8786-028cc4715a1a.png)  
  

*This is a valid PHP code that will return information about your server.*  
You can now access this page in your web browser by visiting your public IP address, followed by /info.php:  
  
`<Public IP>/info.php`  
  
   
 You will see a web page containing detailed information about your server:  
 ![Screenshot_20221207_210139](https://user-images.githubusercontent.com/105195327/206284882-88e5246d-19c8-493e-88c0-dc596a996d51.png)  
  
  
After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:

> `sudo rm /var/www/projectlemp/info.php`  
  
You can always regenerate this file if you need it later.  
  
---
---
## Step 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)  
  


