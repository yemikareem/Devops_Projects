# WebStackImplementation-LEMP_Stack
## Cementing the Skills of Deploying Web Solutions using the LA(E)MP Stacks

### STEP 0: THE PREREQUISITES
- I caught up registering and setting up an AWS free-tier account with Ubuntu Server OS via EC2, [**HERE**](https://github.com/yemikareem/LampStackImplementation#step-0-the-prerequisites)

NB: In the previous project, I used Putty on Windows to connect to EC2 Instance; now I will use the most straightforward option, GitBash.

- To connect to EC2 Instance without converting .pem key to .ppk - using GitBash
  1. [Download and install](https://www.youtube.com/watch?v=qdwWe9COT9k) GitBash
  2. Launched GitBash and ran:
  ```
  ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
  ```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/d2d352f1-8dec-49ba-8ff4-974b105cdb85)

### STEP 1: INSTALLING THE NGINX WEB SERVER
Nginx is a high-performance web server that helps display web pages to site visitors.
- To install Nginx:
  1. I ran 
  ```
  sudo apt update
  ```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/f73c1b57-6273-411b-bdb5-5015a05d1a2c)

  2. Then ran 
  ```
  sudo apt install nginx
  ```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/7a3a623d-4858-434f-8997-91c7bc0da394)

- Tested the Nginx successful file configuration 
```
sudo nginx -t
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/b20b6e1c-d373-40e3-b1e5-4983b3d2aa02)

- Verified that nginx was successfully installed and is running as a service in Ubuntu 
```
sudo systemctl status nginx
```

  NB: It failed to load (as in the image below), I suspected it may be that Apache was installed on the server. 

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/06e2168c-3fd1-4b61-a9c1-e1cbf3497d53)

  Then, I checked if Apache was installed on the server 
  ```
  apachectl -v
  ```


![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/872d4be8-b646-4351-a542-faee02f806b2)

  Yes, it was. So, I uninstalled Apache - 
  ```
  sudo apt-get purge apache2
  ``` 
  then 
  ```
  sudo apt-get autoremove apache2
  ``` 
  then check again with 
  ```
  apachectl -v
  ```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/1ddbb431-d772-47aa-af65-87be2cac19cc)

  Restarted Nginx 
  ```
  sudo systemctl restart nginx
  ```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/0620a163-b326-4074-8b0b-c03eebbf542b)

  Then ran 
  ```
  sudo systemctl status nginx
  ``` 
  over again

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/8b43bd75-6e1b-424f-901e-8caaafa0ba51)

- Ref [Project LampStack](https://github.com/yemikareem/LampStackImplementation#step-1-installing-apache-and-updating-the-firewall): rule already added to EC2 configuration to open inbound connection via TCP port 80 

- Checked how to access it locally in my Ubuntu shell - 
```
curl http://localhost:80
``` 
or 
```
curl http://52.56.228.186:80
``` 

  NB: Another way to retrieve the Public IP Address instead of checking it in the AWS console is by typing this directly in the CLI 
  ```
  curl -s http://169.254.169.254/latest/meta-data/public-ipv4
  ```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/3f6a6f0e-addd-4bea-a741-671524d7d036)

  Well, this was the same content gotten by the 'curl' command but represented in a nice HTML format by the web browser.


### STEP 2: INSTALLING MYSQL
Reference to [**Step 2, Project 1**](https://github.com/yemikareem/LampStackImplementation#step-2-installing-mysql) for a detailed process on how to install MySql.


### STEP 3: INSTALLING PHP
Tip: *Linux** is installed to serve as a work environment, *Nginx** (instead of Apache) is installed to serve the content (this is the main difference in LAMP & LEMP stacks), *MySQL** is installed to store and manage data, and *PHP** processes code and generate dynamic content for the web server.

I needed to install php -fpm , which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. 

I also needed php-mysql , a PHP module that allows PHP to communicate with MySQL-based databases.

Core PHP packages will automatically be installed as dependencies.

- To install these 2 packages at once, I ran: 
```
sudo apt install php-fpm php-mysql
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/fbba2b02-26e1-4b41-8de9-2ad68cd66759)


### STEP 4: CONFIGURING NGINX TO USE PHP PROCESSOR 
Tip: In Nginx web server, server blocks can be created (like virtual hosts in Apache) to contain configuration details and host more than one domain on a single server. 

Using projectLEMP as an example domain name.

On Ubuntu 20.04, Nginx has one server block enabled by default which is configured to serve documents out of a directory at /var/www/html . This will work for a single site, but can become difficult to manage when hosting multiple sites. 

Better not to modify /var/www/html , rather create a directory structure within /var/www for the my_domain website, leaving /var/www/html in place as the default directory to be served if a client request does not match any other sites.

- I created the root web directory for my_domain as follows: 
```
sudo mkdir /var/www/projectLEMP
``` 
- Assigned ownership of the directory with the $USER environment variable, referencing the current system user 
```
sudo chown -R $USER:$USER /var/www/projectLEMP
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/fe1b7206-a437-4f65-9195-e4e74b5e128b)

- Opened a new configuration file in Nginx's sites-available directory, using the preferred command-line editor - I used **nano* 
```
sudo nano /etc/nginx/sites-available/projectLEMP
``` 
this created a new blank file

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/f9309556-d6b0-4bdc-9d3e-5bf93dff3c69)

- Pasted in the below bare-bones configuration - using Ctrl + Right click mouse + Paste

```ruby
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
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/c354017f-becc-478d-96ca-4b4324822aad)

#### WHAT EACH OF THE DIRECTIVES AND LOCATION BLOCKS DO:
- *listen* — Defines what port Nginx will listen on. In this case, it will listen on port *80* , the default port for HTTP.

- *root* — Defines the document root where the files served by this website are stored.

- *index* — Defines in which order Nginx will prioritize index files for this website. It is a common practice to list

- *index.html* files with a higher precedence than *index.php* files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.

- *server name* — Defines which domain names and/or IP addresses this server block should respond for. Point this directive to your server's domain name or public IP address.

- *location /* —Thefirstlocationblockincludesa *try files* directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

- *location ~ \.php$* — This location block handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the *php7.4-fpm.sock file* , which declares what socket is associated with *php-fpm* .

- *location ~ /\.ht* — The last location block dealswith *.htaccess* files, which Nginx does not process. By adding the deny all directive, if any *.htaccess* files happen to find their way into the document root they will not be served to visitors.


When done editing, save and close the file. because we are using nano, we can do so by typing Ctrl+X, then y and ENTER to confirm 

- Activating the configuration by linking to the config file from Nginx's sites-enabled directory, will tell Nginx to use the configuration the next time it is reloaded - "sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/"

- Tested the configuration for syntax error 
```
sudo nginx -t
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/57fa69cf-96e1-427c-a931-f88f6f5483d9)

- There was a need to disable the default Nginx host that was configured to listen on port 80  
```
sudo unlink /etc/nginx/sites-enabled/default
```
- Then reloaded Nginx to apply the changes - 
```
sudo systemctl reload nginx
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/9f39ae72-1f90-445f-96b0-22524c8126ba)

At this point, my new site was active, but the web root /var/www/projectLEMP was still empty. 

- To create an index.html file in that location to test that the new server block was working perfectly: 
```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```

- Now, let's go to the browser to open my website URL using IP address

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/564f8193-f19f-4e85-8275-935abd6e7c0b)

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/27fcc486-c859-42d7-8248-2fc866385d1f)

NB: This can be left in place temporarily as a landing page until index.php is set up for replacement


### STEP 5: TESTING PHP WITH NGINX 
- To test that Nginx can handle .php file to the PHP processor, create a test PHP file in the document root. Open a new file called info.php within the document root in the text editor - "nano /var/www/projectLEMP/info.php"

- Typed or pasted the following:
```ruby 
<?PHP

phpinfo();
```

- When I was done, I saved and closed the file in the text editor. Refreshed the page by adding /info.php to the URL, and the below loaded up:

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/5aa62f0d-bded-4f4d-86d3-26e1c4f7e350)

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/b08dd279-bbf0-4e8f-8064-c6ee1ffe2092)

Please note that it is best to remove the created file after checking the relevant information through this page, as it contains sensitive information about the PHP environment and the Ubuntu server.

- To remove files that contain sensitive information - 
```
sudo rm /var/www/your_domain/info.php
``` 

This can always be regenerated whenever needed. 


### STEP 6: RETRIEVING DATA FROM MYSQL DATABASE WITH PHP
**Objective:** To create a test database (DB) with a simple "To Do List", and configure access to it, so that the Nginx website would be able to query data from the DB and display it. 

NB: Because the native MySQL PHP library *mysqlnd* doesn’t support *caching_sha2_authentication* yet, which is the default authentication method for MySQL 8. We'll need to create a new user with the *MySQL_native_password* authentication method in order to be able to connect to the MySQL database from PHP.

I created a database named *example_database* and a user named *example _user*. On my own, I replaced these names with different values.

- First, I connected to the MySQL console using the root account: 
```
sudo MySQL
```

NB: If not the root user, first enter on the CLI - 
```
sudo mysql -p
```
 then enter the root password

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/c996710b-b7f7-4ff8-babb-e570b22ba949)

- Created a new database on the MySQL console, by running - 
```
CREATE DATABASE `example_database`;
```
  
- Created a new user, by running - 
```
CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

- Granted full access to the user created, by running - 
```
GRANT ALL ON example_database.* TO 'example_user'@'%';
```

- then exited the mysql console

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/dd8eb8c3-dc9c-482c-bf88-cc59d2caa51e)

- I tested if the new user has the proper permissions to login to the mysql console again 
```
mysql -u example_user -p
``` 
then entered the new password

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/a8d05a92-b2c5-40a1-b34b-99349e79274e)

- After logging in to the console, I confirmed that I have access to the example_database database 
```
SHOW DATABASES;
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/91f54cfe-3068-487d-a67a-b75acbbffe65)

- Created a test table named todo_list; from the MySql console, by running - 
```
CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/9070b574-a7e0-4345-8d61-4904e6d8e4b6)

- I inserted a few rows of contents in the test table (this was repeated a few time with different values) 
```INSERT INTO example_database.todo_list (content) VALUES ("My 1st important item");
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/8f4c2531-44aa-403d-937a-3cf0b1581a8d)

- Confirmed that the data was successfully saved to the table 
```
SELECT * FROM example_database.todo_list;
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/9c5e2c30-be5d-4a5d-b350-089417f35d15)
then, exit the console.

- Created the PHP script that connects to MySQL database and query for the content, I then created a new PHP file in the custom web root directory using my preferred editor; in this case I used vi 
```
nano /var/www/projectLEMP/todo_list.php
```

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/3fa4d152-164f-4708-b633-b11d413f8b93)
saved and closed the file when done editing

- The page was now accessible via the web browser using the domain name or public address configured for it adding */todo_list.php*

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/b39a4999-1e16-4ae5-ad98-aab9e6d1fbd6)

![image](https://github.com/yemikareem/WebStackImplementation-LEMP_Stack/assets/141459374/c7772121-f73c-4a5e-a35e-fa9532f3551b)


Project 2: **WebStack Implementation LEMP_Stack**, Completed! 

(c) Yemi Kareem
