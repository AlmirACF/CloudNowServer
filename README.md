# CloudNowServer

Cloud based Project for Murdoch University
Student ID: 355228443
Student Name : Almir Cavalcante Filho

ICT171 - Project Timeline - How to develop a cloud based Blog using EC2 Amazon machine, Route53 domain, Wordpress and Apache.

1) Connecting to my instance to start developing my cloud based server:

You need to open your terminal and find your PEM key file and change directory to that folder and them you connect to your machine using a code like this:

ssh -i "NewmachineKey.pem" ubuntu@ec2-34-201-206-26.compute-1.amazonaws.com

2) To update your Ubuntu system and install Apache:

Update System:

sudo apt update && sudo apt upgrade -y

Installing Apache2:

sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php php-cli php-cgi php-gd -y
sudo systemctl enable apache2
sudo systemctl start apache2

3) Starting MySQL (software that stores and manages structured data in tables using SQL).

sudo systemctl start mysql
sudo systemctl enable MySQL


************* Summarising to good understanding **************

MySQL = brain 🧠 (stores memory and data)

PHP = body 🧍‍♂️ (moves, talks, shows info)

Apache or Nginx = voice 📣 (delivers the website to users)

***************************************************************

4) Creating a Wordpress Database:

Run this code to open MySQL Shell and enter password

sudo mysql -u root -p


The create Wordpress Database using

CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'cloudnow';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;

and now i have created successfully the Wordpress database user and having full access to its database.

5) Download and extracting Wordpress

cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz

You use tmp to create a temporary folder as a short term storage.
Wget - Download the file from its website.
.tar.gz - the file is a compressed archive (e.g zip file).
tar - the command too work with .tar archives
-xzf ( x = extract content / z = uncompress / f specify the file to extract)


6) Moving Wordpress to the web directory

sudo rsync -av wordpress/ /var/www/html/

sudo rm /var/www/html/index.html

7) Configuring permissions

first connect to your database editing your configuration file :

cd /var/www/html
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php

then edit these lines to match what you created for wordpress user / database name / password : 

define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', 'cloudnow' );
define( 'DB_HOST', 'localhost' );

--------------------------------------------------------------------------------------------------------

After this step you are done! Just go to your webpage and select your idiom and create your login: 

http://<your-public-IP>/

When all created click in login or use this link as parameter to login:

http:///<your-public-IP>/wp-login.php

now you are on editor page as Admin and ready to start modifying your Blog page! If you access your domain name you already can see the structure for Wordpress installed.

---------------------------------------------------------------------------------------------------------

I have started my blog selecting a Theme which requested one plug in to be installed:

theme: Avanta

created a logo to use as a Site Icon

Customizing in colors of preference and starting posting using Wordpress.

---------------------------------------------------------------------------------------------------------

Checked with everything is working fine such as:

Domain Name (Route53) : aussietech.click

if SSL encryption is activated : Certified

Wordpress Installed

