# This is LEMP STACK IMPLEMENTATION PROJECT

## Step 1: Installing Nginx Web Server. NGINX is a high-performance web server that will be used for this project. It will be installed using the **apt** package manager

- Before installing nginx, I will start off by updating the server's package index with ***sudo apt udate*** followed by ***sudo apt install nginx*** to install NGINX

![Alt text](<Images/1- sudo update.png>)

- Install Nginx 

![Alt text](<Images/2- Install nginx.png>)

The above images show server's package update and NGINX successful installation.

- The next step is to verify that nginx was successfully installed and running as a service in Ubuntu using ***sudo systemctl status nginx***

![Alt text](<Images/3 - Verify nginx.png>)

- Before the web server can recieve any traffic, TCP Port 80 which is the default port that web browsers use to access web pages, has to be opened. TCP port 22 is open by default for the EC2 Machine to access it via SSH.

- The web server is running and can be accessed locally from the ubuntu shell and from the intenet (Source 0.0.0.0/0) which means from any IP address. 

- The curl command can be used to request Nginx on port 80 via DNS name or I.P (even without specifying port, it will work)

![Alt text](<Images/4-curl DNS.png>)

The image above shows accessing the web server locally throgh DNS

The image below shows accessing the web server locally through IP

![Alt text](<Images/5-curl IP adress.png>)
 
- The output from the curl command showed that the nginx web service responded with some payload. The next thing is to test how the nginx service can respond to request from the internet through the web browser by typing {http://'public IP address':80}

- The public IPV4 address can be found on the instance details in the AWS management console. Alternatively, it can the retrieved with the following command ***curl -s http://169.254.169.254/latest/meta-data/public_IPV4***

![Alt text](<Images/6- curl retrieve public IP.png>)

- The image above shows the output of the curl command which returned the IP Address '13.40.95.17'

- The image below shows the content of the webpage when the nginx service was requested through the web browser.

![Alt text](<Images/7- nginx webpage displayed.png>)

## Step 2: Installing MySQL. Now that the web server is running, a Database Management System (DBMS) needs to be installed to store and mange data for the website in a relational database. MySQL which is a popular relational database management system used within PHP environments will be used for this project. 

- The first step is to use the Apt package manager to acquire and install mysql with ***sudo apt install mysql-server*** command

![Alt text](<Images/8- install mysql.png>)

![Alt text](<Images/9- install mysql complete.png>)

The images above show successful installation of mysql.

- The second step is to login to my sql with the command 'sudo mysql' which will connect MySQL server as the administrative database user **root**. See image below for output.

![Alt text](<Images/10- mysql login.png>)

**NOTE:** It is recommended to run a security script that comes pre-installed with Mysql. This script will remove some insecure default settings and lock down access to the database system. Before running this script, a **root** user password will be set, using "mysql_native_password" as default authentication method. The **root** user password will be defined as "PassWord.1" with the following command: ***ALTER USER 'root'@'local' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';***

- The next step is to exit MySQL shell with ***exit*** command and start the interactive script with ***sudo mysql_secure_installation*** command. This will ask if VALIDATE PASSWORD PLUGGIN wants to be configured. Y will answer Yes and any other key will continue without enabling it.

**NOTE:** For the purpose of this project, VALIDATE PASSWORD PLUGGING was not set.

## Step 3: Installing PHP - nginx has been installed to serve the content and MySQL installed to store and manage the data. PHP is the next to be installed to process the code and generate dynamic content for the web server. 

- Unlike Apache which embeds the PHP interpreter in each request, Nginx requires an external programme to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based website, but it requires additional configuration. It is also required to install **php-fpm**, an acronym for 'PHP FastCGI Process Manager,' to handle PHP requests for processing by Nginx. In addition, **php-mysql**, a PHP module that facilitates communication between PHP and MySQL databases must be installed. Core PHP packages will be automatically installed as necessary dependencies."

- The two packages can be installed at once with the following command ***sudo apt install php-fpm php-mysql*** and press the **Y** key (which means YES)when prompted to confirm installation.

- See images below for PHP installation confirmation and verification respectively.

![Alt text](<Images/14- install php.png>)

![Alt text](<Images/15- verify php.png>)

## Step 4: Configuring Nginx to use PHP Processor - When using the Nginx web server, server blocks (similar to virtual hosts in Apache) can be created to encapsulate configuration details and host more than one domain on a single server. This Project will use "ProjectLEMP" as an example domain name. 

- On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at **/var/www/html**. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instaed of modifying **/var/www/html**, a directory structure will be created within **/var/www** for the new domain **(ProjectLEMP)** website, leaving **/var/www.html** in place as the default directory to be served if a client request does not match any other sites. 

- The first step is to create the web directory for the new domain **ProjectLEMP** with the command ***sudo mkdir /var/www/ProjectLEMP***

See image below: 

![Alt text](<Images/16- making directory project lemp.png>)

- The next step is to assign ownership of the directory with the **$USER** environment variable, which will reference the current user. The following command will be used to assign ownership ***sudo chown -R $USER:$USER /var/www/ProjectLEMP***

See image below:

![Alt text](<Images/17- change ownership.png>)

- In the next step, *nano editor* will be used to open a new configuration file in Nginx's **sites-available** directory using the command ***sudo nano /etc/nginx/sites-available/ProjectLEMP***. 

![Alt text](<Images/18- sudo nano.png>)

This will create a new blank file where the configuration will be written. 

See image below for configuration:

![Alt text](<Images/19- nano configuration.png>)

- Here's what each of these directives and location blocks do: 

**listen** - Defines what port Nginx will listen on. In this case, it will listen on port *80* , the default port for HTTP.

**root** - Defines the document root where the files served by this website are stored. 

**index** - Defines in which order Nginx will prioritize index files for this website. It is a common practice to list **index.html** files with a higher precedence than **index.php** files to allow for quickly setting up a maintenance landing page in PHP applications. You can adjust these settings to better suit your application needs.

**server_name** - Defines which domain names and/or IP addresses this server block should respond for. **Point this directive to your server's domain name or public IP address.**

**location /** - The first location block includes a **try_files** directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate resource, it will return a 404 error.

**Location ~ \.php$** - This location block handles the actual PHP processing by pointing Nginx to the fastegi-php.conf configuration file and the **php7.4-fpm.sock file**, which declares what socket is associated with **php-fpm**.

**Location** ~ /\.ht - The last location block deals with **.htaccess** files, which Nginx does not process. By adding the deny all directive, if any
**.htaccess** files happen to find their way into the document root, they will not be served to visitors.

- The next step is to activate the configuration by linking to the config file from Nginx's **sites-enabled** directory with the following command ***sudo ln -s /etc/nginx/sites-available/ProjectLEMP /etc/nginx/sites-enabled/***

See image below - That will tell Nginx to use the configuration next time it is reloaded.

![Alt text](<Images/20- link ProjectLEMP.png>)

- The next step is to test the Configuration for syntax error with the following command ***sudo nginx -t*** 

See image below for test confirmation which shows that the configuration syxtax has no error.

![Alt text](<Images/21- test syntax error.png>)

- The next step is to disable default Nginx configured to listen on port 80 with the following command ***sudo unlink /etc/nginx/sites-enabled/default*** and reload Nginx to apply the changes. 

See images below:

![Alt text](<Images/22- disable default nginx host.png>)

![Alt text](<Images/23- reload nginx.png>)

- At this point, the website is now active but the root/var/www/ProjectLEMP is still empty. For this project, an index.html file will be created in that location to test the new server block functions as expected. 

See image below for successful creation of index.html file

![Alt text](<Images/24- create index-html.png>)

- The next step is to open the browser to test the website URL using the IP adress by typing __http://"IP-Address":80__

See image below for the text from the ***echo*** command written to the index.html file which means the Nginx site is functioning as expected. The output will display the server's public hostname (DNS name) and public IP address. 

Note: The website can also be accessed from the browser through the public DNS name.  

![Alt text](<Images/25- website displayed.png>)

- At this point, the LEMP stack is now fully configured.

## Step 5: Testing PHP with Nginx - At this point, the LEMP stack is now completely set up and fully operational. 

- The next step is test the Nginx to validate that it can correctly hand **.php** files off to the PHP processor. To do this, a test PHP file will be created in the document root. The nano command will be used to open a new *info.php** file within the document root using the text editor. ***nano /var/www/ProjectLEMP/info.php***

![Alt text](<Images/26- nano php code.png>)

See image below for the code written in the text editor

![Alt text](<Images/27- nano php code.png>)

![Alt text](<Images/28- verify php file.png>)

- The next step is to try and access the page in the web browser by using the domain name or the public IP address followed by **/info.php** 

See image below for the output which shows relevant and sensitive information about the PHP environment and Ubuntu server.  

![Alt text](<Images/40- Output info_php.png>)

**NOTE:** The page that contains sensitive information about the server can be removed with the following command. ***sudo rm /var/www/domain-name/info.php***  

## Step 6: Retrieving data from MySQL database with PHP. In this step, a test database will be created with a simple "To do list" and configure access to it, so the Nginx website would be able to querry data from the Database and display it.

- The next step is to create a new user with the **mysql_native_password** aithentication method in order to be able to connect to MySQL database from PHP. An example database and a test-user will be created namely Demo_DB and Demo-User respecively. To do this, I will connect to MySQL console through the **root** account;

See image for successful root login.

![Alt text](<Images/29 mysql sudo login.png>)

- The next step is to create the database with the following command ***CREATE DATABASE Demo-DB;***

![Alt text](<Images/30- create database.png>)

- The next step is to create **Demo-User** and grant full privileges to the user using *mysql_native_password** as default authentication method to define the password as **PassWord.1**

The image below shows the command used to create **Demo-User** 

![Alt text](<Images/31- Create User.png>)

- The next step is to grant permission to the user over the database and then exit MySQL after granting the permission. 

See image below for the command that gives **Demo-User** full privileges over **Demo_DB** while preventing the User from crreating or modifying other databases on the server.

![Alt text](<Images/32- Grant user permission.png>)

- The next step is to logging to MySQL console agqin with **Demo-User** credentials to test that the user has the proper permissions. 

See image below for successful Demo-User login:

![Alt text](<Images/33- Test User permission okay.png>)

- The next step is to confirm that **Demo-User** has access to **Demo_DB** with the command ***SHOW databases;*** 

See image below for the output.

![Alt text](<Images/34- show databases.png>)

- The next step is create a table named **todo_list**. See image below for the command.

![Alt text](<Images/36- create test table.png>)

![Alt text](<Images/37- create to do list.png>)

- The next step is to confirm that the data was successfuly save by using the following command ***SELECT * FROM Demo_DB.todo_list;*** and exit MySQL console after the confirmation.

See image below for the output

![Alt text](<Images/38- confirm data saved in to do list.png>)

- The next step is to use the **nano** command to create a PHP script in the text editor that will connect to MySQL and querry for the content using the following command ***nano /var/www/ProjectLAMP/todo_list.php***

See image below for the content of the script in the text editor before saving the script.

![Alt text](<Images/39- Nano config.png>)

- The next step is to access the page in a web browser by visiting the domain name or public IP configured for the website, followed by **/todo_list.php** 

See image below for the output of **http://3.9.14.137/todo_list.php**

![Alt text](<Images/41- Todo list Successful.png>)

- The image above confirms that the PHP environment is ready to connect and interact with MySQL server.

# This is the end of LEMP STACK PROJECT. 











































