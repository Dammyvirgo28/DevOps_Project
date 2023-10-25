# This is Web stack Implementation (LAMP STACK)

## Step 1 - Installing Apache 

- Apache HTTP Server is the most widely used Web server. The first step is to use the Ubuntu's package manager 'apt' to install Apache on the EC2 instance that was provisioned on Amazon Web Service and update the firewall

- To install Apache 'sudo apt install apache2'

![Alt text](<Images/Install Apache2.png>)

![Alt text](<Images/Installing Apache2 Confirmation.png>)

- To update the firewall, 'sudo apt update'

![Alt text](<Images/Sudo apt update.png>)

- To confirm that apache2 is running, 'sudo systemctl status apache2'

![Alt text](<Images/Verify Apache2.png>)

- Before the web server can recieve any traffic, TCP Port 80 which is the default port that web browsers use to access web pages has to be opened. TCP port 22 has been opened by default to enable access via SSH, but EC2 configuration rule has to be added to open inbound connection through port 80.


- To test local access on Ubuntu shell 'curl http://localhost:80' or 'curl http://127.0.0.1:80' can request the Apache HTTP server over port 80.

![Alt text](<Images/Curl-Test port 80.png>)

- The image above shows that the Apache server responded to the 'curl' command with some payload.

## Step 2 - Installing MySQL

- Now that the web server is up and running, the next thing is to install a *Database Management Sytem (DBMS)* for data storage and management in a relatioanl database. MySQL is a relational database management system used within PHP environments. Again, the Ubuntu's package manager 'apt' will be used to install MySQL.

- To install MySQL 'sudo apt install mysql-server' and confirm installation by typing 'Y' and press the 'enter' key

![Alt text](Images/MySQL-Installing.png)

![Alt text](<Images/MySQL-Install Complete.png>)


















