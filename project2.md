## WEB STACK IMPLEMENTATION (LEMP STACK)
-Downloaded and Installed Git Bash.

-Launched Git Bash ran the command:

`ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>`

### Blocker encountered
When i ran the command as stated above, i got back an error message:

`Warning: Identity file new_ubuntu.pem not accessible: No such file or directory.`

![a](https://user-images.githubusercontent.com/81443311/113779396-8c65d480-9725-11eb-9672-d6b9dd8fd1c2.png)

### Solution to Blocker
After a bit of research, I discovered the needed to put the path file path of my .pem key in the command.

![b](https://user-images.githubusercontent.com/81443311/113779656-f4b4b600-9725-11eb-8893-9aa210ecae5a.png)

### Step1
 #### Installing the Nginx Web Server
-Updated the server’s package index

-Used the apt command to install Nginx. 

`$ sudo apt update`

`$ sudo apt install nginx`

-Confirmed that Ngix has successfully installed on the Ubuntu server.

![c](https://user-images.githubusercontent.com/81443311/113780368-064a8d80-9727-11eb-80cc-529504c647a8.png)

-Opened TCP port 80 on Ubuntu server

![d](https://user-images.githubusercontent.com/81443311/113780744-88d34d00-9727-11eb-8be7-1411d70fe806.png)

-Checked to see if Server can be accessed locally in Ubuntu shell, i ran the command:

`curl http://localhost:80` 

![e](https://user-images.githubusercontent.com/81443311/113780608-5aee0880-9727-11eb-9529-35ef3577f73f.png)

Tested Nginx to see if it responds to requests from the internet.

-opened web browser and accessed the url

`http://3.129.59.174:80`

![f](https://user-images.githubusercontent.com/81443311/113781025-f3848880-9727-11eb-9e83-c7474dbff128.png)


### STEP 2 INSTALLING MySQL

-Installed mysql using command

`sudo apt install mysql- server`

`$ sudo mysql_secure_installation`


-Tested if able to log in to the MySQL console by running 

`sudo mysql`


![h](https://user-images.githubusercontent.com/81443311/113781343-79083880-9728-11eb-9801-352a1395b711.png)

![i](https://user-images.githubusercontent.com/81443311/113782009-6e9a6e80-9729-11eb-8901-2131c01f2ad7.png)

-Exited MySQL console with:
                
`mysql> exit`

### STEP 3 INSTALLING PHP

-Ran command to install the duo packages (Php-fpm and php-mysql) 

`sudo apt install php-fpm php-mysql`

![j](https://user-images.githubusercontent.com/81443311/113782177-af928300-9729-11eb-8d8c-f7b351807fce.png)

### STEP 4 CONFIGURING NGNIX TO USE PHP PROCESSOR

-Created a root web directory for your_domain
`sudo mkdir /var/www/projectLEMP`

-Assigned ownership of directory with $USER environment variable

`sudo  chown -R $USER:$USER /var/www/projectLEMP`

![K](https://user-images.githubusercontent.com/81443311/113782383-039d6780-972a-11eb-82a2-b632812c721b.png)

-Opened a new configuration file in Nginx’s directory (nano text editor used)

`sudo nano /etc/nginx/sites-available/projectLEMP`

-Pasted the following commands into the blank file.

```
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }
}
```

![L](https://user-images.githubusercontent.com/81443311/113783339-8246d480-972b-11eb-800b-05bf49b00d4e.png)

-Saved file and activated the configuration by linking to the config file from Nginx’s directory. Using this command:

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

-Tested for Syntax error with :

`sudo nginx -t`
  
-Diasabled default Nginx host that was configured to run on port80 .

`sudo unlink /etc/nginx/sites-enabled/default`


![M](https://user-images.githubusercontent.com/81443311/113783416-9db1df80-972b-11eb-9b91-89c74e3cd29f.png)

-Reloaded Nginx to apply changes:

`$ sudo systemctl reload nginx`

-Index.html file was created in the location web root/var/www/projectLEMP in order to test if the new server block works as expected.

-Opened website url on the browser using IPaddress

`http//13.58.23.4:80`

![N](https://user-images.githubusercontent.com/81443311/113783581-dd78c700-972b-11eb-985b-69c6f468bf88.png)


### STEP 5 TESTING PHP WITH NGINX

-Created a test PHP file in the document root called info.php

`nano var/www/projectLEMP/info.php`

-Pasted PHP code into the new file.
```
<?php
phpinfo();
```
![O](https://user-images.githubusercontent.com/81443311/113783903-63950d80-972c-11eb-9777-5f4ac8f159a9.png)

-Accessed the page by visiting the ip address followed by /info.php

![P](https://user-images.githubusercontent.com/81443311/113784001-89baad80-972c-11eb-8117-a5b8954fabe6.png)

-Removed the .php file created 

`sudo rm /var/www/projectLEMP/info.php`

### STEP 6 RETRIEVING DATA FROM MySQL DATABASE WITH PHP

-Connected to MySQL

`sudo mysql`

-Created a new database by running the command:

`mysql> CREATE DATABASE `example_database`;`

-Created a new user and granted full privileges on the database created.

`mysql>  CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';`

-Gave user full permission over database by running cmd

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

-Exited  mysql shell

![T](https://user-images.githubusercontent.com/81443311/113785314-c4254a00-972e-11eb-8112-2fe0304af1bb.png)

-Tested if new user has permissions by logging into mysql console again with

`$ mysql -u example_user -p`

![U](https://user-images.githubusercontent.com/81443311/113785649-74934e00-972f-11eb-8c89-39ae2026023d.png)
 
-Ran cmd 

`mysql> SHOW DATABASES;`

-Created a todo_list by running the statement

```
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
```
![V](https://user-images.githubusercontent.com/81443311/113786150-7c072700-9730-11eb-8da8-e2aaebea7971.png)


-Inserted a few rows of content by running this command several times with various values.

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

![W](https://user-images.githubusercontent.com/81443311/113786313-cab4c100-9730-11eb-80f7-981622e457b1.png)


-Confirmed data was successfully saved  by running

`mysql>  SELECT * FROM example_database.todo_list;`

![X](https://user-images.githubusercontent.com/81443311/113786393-f33cbb00-9730-11eb-8b6c-f36110a4f7ac.png)

-Exit MySQL.


-Created a PHP script that will connect to MySQL and query content by creating a php file in the custom web root directory  using nano editor.

`$ nano /var/www/projectLEMP/todo_list.php`

-This script was then copied into the todo_list.php file

```
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```

### BLOCKER 

-Ran the script as it was:

![xxx](https://user-images.githubusercontent.com/81443311/113786817-d1900380-9731-11eb-8f6e-f250b0918154.png)

- Got error message back when i tried accessing the URL.

![xxxx](https://user-images.githubusercontent.com/81443311/113786888-f2f0ef80-9731-11eb-984c-f5c1c1a7156a.png)

### SOLUTION TO BLOCKER

- Changed password in script to password set for the MySQL account

![Y](https://user-images.githubusercontent.com/81443311/113787154-65fa6600-9732-11eb-833f-78c6d6aeea82.png)


-Accessed the page in web browser by accessing the link

`http://<Public_domain_or_IP>/todo_list.php`

![Z](https://user-images.githubusercontent.com/81443311/113787189-74e11880-9732-11eb-8a4b-53fd4fcdfa51.png)











