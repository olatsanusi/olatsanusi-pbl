## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
SDLC
Software development life cycle.also referred to as the Application Development Life-Cycle, is a process for planning, creating, testing, and deploying an information system.It consists of a detailed plan describing how to develop, maintain, replace and alter or enhance specific software. 
There are usually six stages in this cycle: Requirement Analysis, Design, Development and Testing, Implementation, Documentation, and Evaluation.

THE LAMP Stack
This is an example of a web service solution stack, named as an acronym of the names of its four components (which are open-source);  Linux OS,Apache HTTP server, the MySQL (relational database management system (RDBMS)), and the PHP programming language. The LAMP components are largely interchangeable and not limited to the original selection. As a solution stack, LAMP is suitable for building dynamic web pages and web applications.

TCP- Transmission control protocol is a connection oriented protocol where connection between client and server needs to be established before data can be sent.It relies on the 3-way handshake rule for transmission to be established. Major internet applications such as the web,email, file transfer and remote connection rely on TCP.

UDP- User datagram Protocol is a type of internet protocol suite where computers send messages to other hosts on an IP network without prior connection being established. It is a connectionless oriented transfer with minimum check is required and no connection is established before transfer can begin.


Most common network  ports are-
SSH- 22
TELNET- 23
SMTP(simple mail transfer Protocol)- 25
DNS- 53
HTTP- 80
POP3-110
FTP Data transfer - 20
FTP Command Control - 21
HTTPS- 443
RDP- 3389


Preparing prerequisites
Created an AWS account.

Launched EC2 instance with ubuntu server 20.04 LTS (HVM) provisioned.

![1](https://user-images.githubusercontent.com/81443311/113040991-59ee3180-9191-11eb-8c80-fb8cc5c5d767.png)

-Connected to EC2 instance using putty. 
a.The private key saved (.pem file) needed to be converted to a .ppk format using the puttyGen.
b.Using putty,the saved private key and the EC2 public IP address are used to establish connection between the EC2 instance and client (me).

# STEP 1- Installing Apache and updating firewall steps:

Ran Ubuntu Package manager ‘apt’ 
Verified apache is running as a service in the OS.

![2 (2)](https://user-images.githubusercontent.com/81443311/113041416-da149700-9191-11eb-8d0b-6c8c3fbb2363.png)

3.Opened TCP port 80 on EC2 instance

4.Enabled firewall on the server with command sudo ufw enable
 and Allowed Apache traffic sudo ufw allow "Apache"

5.Checeked to see if Ubuntu shell can be access locally by running the command
  curl http://localhost:80

![3](https://user-images.githubusercontent.com/81443311/113041596-0e885300-9192-11eb-8566-149a2c6e670b.png)

6.confirmed Ubuntu default page

![4](https://user-images.githubusercontent.com/81443311/113041758-3e375b00-9192-11eb-8545-eadf44c5fa83.png)


# Step2 - INSTALLING MYSQL

Used $ sudo apt install mysql-server command to install mysql.

![5](https://user-images.githubusercontent.com/81443311/113042021-95d5c680-9192-11eb-8695-e96e1292b4b8.png)

Followed steps
Ran command $ sudo mysql. 
Confirmed MYSQL server  installed.
![6](https://user-images.githubusercontent.com/81443311/113042308-eea55f00-9192-11eb-96d0-6e47a08bfe3c.png)

# STEP 3 - INSTALLING PHP 

Install 3 packages of PHP at once with command -
 $ sudo apt install php libapache2-mod-php php-mysql.
Confirm version with command - php -v

![7](https://user-images.githubusercontent.com/81443311/113042459-201e2a80-9193-11eb-993f-d61b670ef75f.png)


# STEP 4- Creating a virtual host for website using Apache

-create directory for project lamp
$ sudo mkdir /var/www/projectlamp
-Assign ownership to the directory with $USER
$ sudo chown -R $USER:$USER /var/www/projectlamp
-created new configuration file using Vi editor
-Pasted configuration into a new blank file created and saved it.
- used the -ls command to show new file - $ sudo ls /etc/apache2/sites-available

![8](https://user-images.githubusercontent.com/81443311/113042721-6e332e00-9193-11eb-8b2a-b25bb7c5c4ad.png)

-ran command to enable new virtual host
$ sudo a2ensite projectlamp

-Disabled Apache's default website i ran command
$ sudo a2dissite 000-default
-confirmed there were no syntax errors.
-Reloaded Apache for changes to take effect.
- copied an index .html file into projectlamp to test the virtual host.

![9](https://user-images.githubusercontent.com/81443311/113042866-9ae74580-9193-11eb-9bcd-11cf6895aff6.png)


- Opened website URLwith IP address.

![10](https://user-images.githubusercontent.com/81443311/113042985-bf432200-9193-11eb-8ccc-99889b939215.png)

# STEP 5 - Enable PHP on website

-Changed the order in which index.php file is listed with DirectoryIndex directive.
-Reloaded Apache for the changes to take effect.
-Created a new PHP script to test that PHP is correctly installed.This was done by:

1)creating a new file index.php inside the custom root folder.

$ vim /var/www/projectlamp/index.php

2) Paste command 

<?php
phpinfo();

Refresh the page. New PHP info page is brought up.

![11](https://user-images.githubusercontent.com/81443311/113044110-37f6ae00-9195-11eb-8dae-24caf76ccba4.png)

-Removed this PHP file with the command .$ sudo rm /var/www/projectlamp/index.php







