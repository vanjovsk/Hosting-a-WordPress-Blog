# Hosting-a-WordPress-Blog
steps to host a wordpress blog


I performed the following tasks:

Connected to an EC2 instance to download and install software packages.

Created an SSL certificate for the Webserver.

Downloaded the latest WordPress package and installed WordPress.

Installed and configured a MariaDB database server.

Installed the PHP graphics drawing library on an Amazon Linux image.

<h1>Task 1: Connect to EC2 instances  using Sessiong manager, go to the management console, choose ec2, instances running, slecet the instance and connect, in the session manager> connect.</h1>

Task 1.2: Downloading software packages for setting up database and web server. a database server and for setting up TLS on Amazon Linux 2023 instance with a self-signed digital certificate, I run the following command:

sudo dnf install wget php-mysqlnd httpd php-fpm php-mysqli mariadb105-server php-json php php-devel openssl mod_ssl -y

######

Task 1.3: download the latest WordPress installation package, run the following command from the shell prompt (downloading lasted wordPress package):

wget https://wordpress.org/latest.tar.gz


*To unzip and unarchive the installation lackage, run this command from the shell prompt:

tar -xzf latest.tar.gz

######
Task 1.4: Configuring SSL/TLS on Amazon Linux 2023 (process of setting up TLS on Amazon Linux 2023 with a self-signed digital certificate:

Additional information: Refer to Configure SSL/TLS on Amazon Linux 2023 in the Additional resources section for more information.
This will connected to the EC2 instance, downloaded and installed the software packages and set up TLS with a self-signed digital certificate.


<h1> Task 2:Creating a database user and database for WordPress installation </h1>
The WordPress installation needs to store information, such as blog posts and user comments, in a database. MariaDB is used as the database. here, i  created a blog’s database and a user authorized to read and save information to it.

To start the database and web server:

sudo systemctl start mariadb httpd

To log in to the database server as the root user:

sudo mysql -u root -p

<img width="908" height="460" alt="Screenshot 2026-01-13 at 12 20 40 AM" src="https://github.com/user-attachments/assets/f89d4128-52ae-43cf-952a-ff7efa1685f8" />

Create a user and password for the database, run the following command from the MariaDB shell prompt:


CREATE USER 'wordpress-user'@'localhost' IDENTIFIED BY 'Welcome@123';

 Create your database, run the following command from the MariaDB shell prompt:

 CREATE DATABASE `wordpress-db`;

Grant full privileges for the database to the WordPress user:

GRANT ALL PRIVILEGES ON `wordpress-db`.* TO "wordpress-user"@"localhost";

Flush the database privileges in order to pick up all the changes and exit:
FLUSH PRIVILEGES;
exit



<h1>Task 3: Creating and editing the wp-config.php file </h1>

Copy the wp-config-sample.php file to a file called wp-config.php:

cp wordpress/wp-config-sample.php wordpress/wp-config.php

Edit the wp-config.php file in nano text editor:

nano wordpress/wp-config.php


Using the keyboard arrow keys to move the cursor, locate the section called Authentication Unique Keys and Salts.
Move the cursor to the start of the first line of the first define key value, in the d of define, and press Ctrl+K to cut the line.
Repeat the procedure to cut the remaining seven lines defining the key and salt values.
Visit https://api.wordpress.org/secret-key/1.1/salt.
Copy (Ctrl-C or right-click & copy) the entire key and salt values generated.
Return to the wp-config.php file opened with the nano text editor.
Right-click and choose Paste from the context menu to paste the random key and salt namespace values generated from the WordPress site.
Enter Ctrl-X to exit.
Enter Y to save modified buffer.
Press Enter to write to the wp-config.php file.
these steps will make you successfully edited the WordPress configuration file to fit your specific configuration.

######
<h1>Installing WordPress files under Apache document root</h1>
Now I already unzipped the installation folder, created a database and user, and customized the WordPress configuration file, ready to copy  installation files to web server document root so I can run the installation script that completes the installation.The location of these files depends on whether you want your WordPress blog to be available at the actual root of your web server (for example, my.public.dns.amazonaws.com) or in a subdirectory or folder under the root (for example,my.public.dns.amazonaws.com/blog).
this will make you have successfully copied the installation files to your web server document root in order for the installation script to completes the installation. 



create an alternative directory named blog under the document root and to copy the files to that directory:

sudo mkdir /var/www/html/blog
sudo cp -r wordpress/* /var/www/html/blog/

<h1> Task 5: Allowing WordPress to use permalinks </h1>

Edit the httpd.conf file in nano text editor:
sudo nano /etc/httpd/conf/httpd.conf

using the keyboard arrow keys to move the cursor, locate the section that starts with <Directory “/var/www/html”>.
hange the AllowOverride None line in the <Directory “/var/www/html”> section to read AllowOverride All .

Press Ctrl+X to exit.

Enter Y to save modified buffer.

Press Enter to write to the httpd.conf file.

this will make you  successfully configured WordPress to use Permalinks.

#####

<h1>Task 6: Installing the PHP graphics drawing library on the Amazon Linux image</h1>
install the Graphics Draw (GD) library for PHP which enables you to modify images.


Install the PHP graphics drawing library:
sudo dnf install php-gd

<img width="1735" height="850" alt="Screenshot 2026-01-13 at 12 49 03 AM" src="https://github.com/user-attachments/assets/bd488fc8-e1b5-42e4-8704-c07b36e80d7c" />

Enter y when prompted during the install to complete installation

<img width="1722" height="237" alt="Screenshot 2026-01-13 at 12 49 42 AM" src="https://github.com/user-attachments/assets/77e7c867-e8dd-4597-8fa4-f527bb676411" />

verify the installed version:
sudo dnf list installed | grep php8.3-gd

this is how you install GD library for PHP which enables you to modify images.




<h1>Task 7: Configuring WordPress file permissions for Apache web server</h1>
Some of the available features in WordPress require write access to the Apache document root. configure the Apache web server files to allow write access to WordPress features.

Grant file and group ownership of /var/www and its contents to the apache user:
sudo chown -R apache /var/www
sudo chgrp -R apache /var/www

Change the directory permissions of */var/www and its subdirectories to add group write permissions and to set the group ID on future subdirectories:
sudo chmod 2775 /var/www
find /var/www -type d -exec sudo chmod 2775 {} \;

Recursively change the file permissions of /var/www and its subdirectories:
find /var/www -type f -exec sudo chmod 0644 {} \;





