# This is LEMP STACK IMPLEMENTATION PROJECT

## Step 1: Installing Nginx Web Server. NGINX is a high-performance web server that will be used for this project. It will be installed using the **apt** package manager

- Before installing nginx, I will start off by updating the server's package index with ***sudo apt udate*** followed by ***sudo apt install nginx*** to install NGINX

- ![Alt text](<Images/1- sudo update.png>)

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

- On Ubuntu 20.04, Nginx has one server block enabled by default and is configured to serve documents out of a directory at */var/www/html*. While this works well for a single site, it can become difficult to manage if you are hosting multiple sites. Instaed of modifying **/var/www/html**, a directory structure will be created within */var/www* for the new domain **(ProjectLEMP)** website, leaving */var/www.html* in place as the default directory to be served if a client request does not match any other sites. 






















































































