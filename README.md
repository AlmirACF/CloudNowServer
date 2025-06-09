# CloudNowServer

Cloud based Project for Murdoch University

Student ID: 355228443

Student Name : Almir Cavalcante Filho

ICT171 - Project Timeline - How to develop a cloud based Blog using EC2 Amazon machine, Route53 domain, Wordpress and Apache.

Step by step of how to set up a complete Virtual Machine with Amazon AWS EC2 (Elastic IP, Domain via Route53, WordPress and SSL Certificate with Let’s Encrypt).


_____________________________________________________________________________________________________________________________________________________________________________________________

### PART 1 Creating and seting up you new Instance on Amazon EC2 (With Elastic IP, Domain Name and SSL Certificate) 

_____________________________________________________________________________________________________________________________________________________________________________________________



### 1. Start creating an account in AMAZON AWS with credit card linked to it

   link:  http://aws.amazon.com/ec2/

   - login in -> go to EC2 page -> Launch Instance
 
   - Choose a name for your Virtual Machine -> select Ubuntu -> and you can use the free tier machine for studying purposes (750hr / month free for 1 year)

   - Configure the instance details -> You can leave everything as the default -> the default is to create a virtual machine with an 8GB hard drive. That's fine, leave it at the default.

   - There is already SSH rule added to access the machine -> click on add rule and select HTTP (Web) as type (This allows web requests to be received by our server).

   - Now click on create a new key pair, choose a name to identify your machine and save it safely.
     
   - And you are ready to Launch it! :)
     

     > ⚠️ IMPORTANT : The Key you have created to your machine is requested everytime you want to access to your machine.


### 2. Log in to your machine using the command line

   Now that your server is running in the cloud you need to login to the command line of your virtual machine.

   Select your machine and click Connect, one similar code like this will be provided to you: 

    ssh -i "yourkeyname.pem" ubuntu@ec2-12-123-1-35.ap-southwest-5.compute.amazonaws.com

   Now open your command line and use the cd (Change Directory) to move to where your machine key was saved on your computer

   Inside the page you can confirm using ls to check all the files on that directory and then copy and paste the connect code to access your machine

   You're in! Once you have command-line access to your virtual machine, the apt repositories may be out of date. Before you install anything, it is often a good idea to update them with :

     sudo apt update 

     sudo apt install apache2



### 3. How to create an elastic IP and link it to your existing machine


An Elastic IP is a static public IP address that you can associate with your EC2 instance. This prevents the IP from changing when you stop and start the instance.


Steps:

- Go to the AWS Console → EC2

- In the sidebar, click "Elastic IPs"

- Click “Allocate Elastic IP address”

- Leave the default option ("Amazon pool of IPs")

- Click "Allocate"

- You’ll see the IP created. Now click “Actions → Associate Elastic IP address”

- Under "Resource type", select "Instance"

- Choose your EC2 instance

- Click "Associate"

- DONE! Now your instance has a permanent public IP address.





### 4. How to buy a domain name with Route 53 and link to your existing machine:


- Go to the AWS Console → Route 53

- Click “Domain Registration → Register Domain”

- Choose and purchase a domain (e.g., my domain costs USD 3 / year  - aussietech.click)

- After the domain is registered, go to “Hosted Zones” and click your domain

- Click "Create Record":

- Record name: leave blank (for root domain)

- Record type: A – IPv4 address

- Value: enter your Elastic IP

- TTL: 300 (or leave default)

- Click "Create records"

- Your domain now points to your EC2 instance.



### 5. How to Install an SSL Certificate with Let’s Encrypt (HTTPS):

SSL is essential for security and removes the “Not Secure” warning in browsers. We'll use Certbot with Apache.

Steps:
On your EC2 instance terminal:


```bash
sudo apt update

sudo apt install certbot python3-certbot-apache -y
```


Run Certbot:

```bash
sudo certbot --apache
```

Follow the prompts:

Enter your email

Accept the terms

Certbot will detect your domain (make sure it's already pointing to your IP)

Choose the option to redirect all HTTP traffic to HTTPS



(Optional) Test auto-renewal:

```bash

sudo certbot renew --dry-run
```

Done! Your website is now secured with a valid SSL certificate from Let’s Encrypt.


_____________________________________________________________________________________________________________________________________________________________________________________________

## PART 2 Connecting to your Instance install WordPress, configure permissions, MySQL and check if your page is working
_____________________________________________________________________________________________________________________________________________________________________________________________


### 6. Connecting to my instance to start developing my cloud based server:

You need to open your terminal and find your PEM key file and change directory to that folder and them you connect to your machine using a code like this:


```bash
ssh -i "NewmachineKey.pem" ubuntu@ec2-34-201-206-26.compute-1.amazonaws.com
```

### 7. To update your Ubuntu system and install Apache, MySql and PHP:

Update System:

```bash
sudo apt update && sudo apt upgrade -y
```

Installing Apache2, Mysql and PHP:
```bash

sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php php-cli php-cgi php-gd -y

sudo systemctl enable apache2

sudo systemctl start apache2
```

### 8. Starting MySQL (software that stores and manages structured data in tables using SQL).
```bash

sudo systemctl start mysql

sudo systemctl enable mysql
```

## ***** Summarising to good understanding *****

*MySQL = brain 🧠 (stores memory and data)*

*PHP = body 🧍‍♂️ (moves, talks, shows info)*

*Apache or Nginx = voice 📣 (delivers the website to users)*

## *********************************************


### 9. Creating a Wordpress Database

Run this code to open MySQL Shell and enter password:

```bash
sudo mysql -u root -p
```

Create Wordpress Database using:

``CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'cloudnow';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;``

and now i have created successfully the Wordpress database user and having full access to its database.

### 10. Download and extracting Wordpress

```bash

   cd /tmp

   wget https://wordpress.org/latest.tar.gz

   tar -xzf latest.tar.gz
```


Explaining the code: You use tmp to create a temporary folder as a short term storage :

Wget - Download the file from its website.

.tar.gz - the file is a compressed archive (e.g zip file).

tar - the command too work with .tar archives

-xzf ( x = extract content / z = uncompress / f specify the file to extract)


### 11. Moving Wordpress to the web directory

```bash

sudo rsync -av wordpress/ /var/www/html/

sudo rm /var/www/html/index.html
```

### 12. Configuring permissions

first connect to your database editing your configuration file :

```bash
cd /var/www/html
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```

then edit these lines to match what you created for wordpress user / database name / password : 

```
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', 'cloudnow' );
define( 'DB_HOST', 'localhost' );
```


After this step you are done! Just go to your webpage and select your idiom and create your login: 

``
http://<your-public-IP>/
``

When all created click in login or use this link as parameter to login:

``
http:///<your-public-IP>/wp-login.php
``

Now you are on editor page as Admin and ready to start modifying your Blog page! If you access your domain name you already can see the structure for Wordpress installed!

---------------------------------------------------------------------------------------------------------

**I have started my blog selecting a Theme which requested one plug in to be installed:**

```
theme: Avanta

created a logo to use as a Site Icon

Customizing in colors of preference and starting posting using Wordpress.
```

---------------------------------------------------------------------------------------------------------

Check if everything is working fine as expected:

- [ ] Amazon EC2 instance running

- [ ] Domain Name (Route53) : e.g. aussietech.click

- [ ] SSL encryption is activated : Certified

- [ ] Wordpress Installed


_____________________________________________________________________________________________________________________________________________________________________________________________

### PART 3 Scheduling your Backup for your blog!

_____________________________________________________________________________________________________________________________________________________________________________________________



### 13. SSH into your EC2 instance and create a backup bash script using this code below:

```bash
nano ~/backup_wordpress.sh
```

**You are going to copy and paste this code to create a Bash script designed to back up your WordPress site and database on an Ubuntu server (like your EC2 instance).**

Create a timestamped backup for:

- Your WordPress files (i.e. themes, plugins, media, etc.)

- Your MySQL database (site content, posts, settings)

- Defines and creates a directory to store backups (if it doesn't exist already).

- Compresses the entire /var/www/html directory (where WordPress is installed) into a .tar.gz archive.

- Store and save the MySQL database named wordpress into a .sql file.

- Compresses the database dump and deletes the raw .sql file to save space.

- Prints with Echo a confirmation message when backup finishes.


```bash
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M")
BACKUP_DIR="/home/ubuntu/backups"
mkdir -p "$BACKUP_DIR"


tar -czf "$BACKUP_DIR/wp-files-$TIMESTAMP.tar.gz" /var/www/html

(Backup WordPress database)

sudo mysqldump -u root wordpress > "$BACKUP_DIR/wp-db-$TIMESTAMP.sql"
tar -czf "$BACKUP_DIR/wp-db-$TIMESTAMP.tar.gz" "$BACKUP_DIR/wp-db-$TIMESTAMP.sql"
rm "$BACKUP_DIR/wp-db-$TIMESTAMP.sql"

echo "Backup completed successfully at $TIMESTAMP"
```




(change permissions for script and making it executable)

```bash
chmod +x ~/backup_wordpress.sh
```

(you can test the script)

```bash
./backup_wordpress.sh
```

### 14. Open the notepad and use this code to create a powershell script to your virtual machine:

**This is a PowerShell script designed to automate backing up your WordPress site from an EC2 instance to your local Windows machine.**


Step 1: Variable definitions (Your actual values)
Step 2: SSH -> Trigger backup script on EC2
Step 3: SCP -> Download latest backup

(You need to replace file path and IP for you values)

```bash
$KeyPath = "C:\Users\YourName\path\to\NewmachineKey.pem"
$RemoteUser = "ubuntu"
$RemoteHost = "34.201.206.26"
$LocalSavePath = "C:\Backups"


ssh -i $KeyPath "$RemoteUser@$RemoteHost" "bash ~/backup_wordpress.sh"

scp -i $KeyPath "$RemoteUser@$RemoteHost:~/backups/*.tar.gz" $LocalSavePath
```


**⚠️ IMPORTANT: Save it as All files type and the file name ending with  ".ps1" to this location: C:\Backups)**
```
C:\Backups\ec2-backup.ps1
```


### 15. Now we can manage the script to do the backup, desactivate it or schedule a backup period


**Schedule to backup every 15 days at the same time (You can customize your backup schedule as needed)**

```bash
schtasks /create /sc daily /mo 15 /tn "CloudBackup" /tr "powershell.exe -ExecutionPolicy Bypass -File C:\Backups\ec2-backup.ps1" /st 15:00
```

Meaning of the code:

```
/sc daily /mo 15                     -> every 15 days -> you can modify if you want

/tn "CloudBackup"                    -> name of the task

/tr ...                              -> runs the PowerShell script

/st 15:00                            -> runs at 3:00 PM you can modify if you want
```


**Running the backup manually**

```bash
powershell -ExecutionPolicy Bypass -File "C:\Backups\ec2-backup.ps1"
```

**Disable task**

```bash
Disable-ScheduledTask -TaskName "CloudBackup"
```

**Check Task**

```bash
schtasks | findstr CloudBackup
```

_____________________________________________________________________________________________________________________________________________________________________________________________


### License : This project, CloudNow, is licensed under the Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0).

_____________________________________________________________________________________________________________________________________________________________________________________________


The primary purpose of this project is educational. It was developed as part of a university assignment in cloud computing, hosted on an Amazon EC2 instance with a custom domain and WordPress setup. As such, the license was selected to ensure the work remains available for learning, inspiration, and academic sharing-while protecting it from unauthorized commercial exploitation.

**Key Benefits:**

   - Attribution Encouraged
This license allows others to share, adapt, and build upon the work only if they provide appropriate credit. This supports academic integrity and highlights the original author (Almir Cavalcante Filho) as the creator, building a professional presence.

   - Non-Commercial Use Only
The license restricts usage to non-commercial contexts. This means educators, students, and developers may learn from or replicate the work for personal, academic, or portfolio purposes—but may not sell, redistribute commercially, or integrate it into paid services without explicit permission.

   - Open Educational Resource (OER) Friendly
By choosing this license, the project aligns with the principles of open educational resources (OER), supporting open access to knowledge in cloud computing technologies like EC2, Route 53, Apache, and WordPress.


### Bonus Step : Adding License to your WordPress Blog Page:

   - Choose a good license for your page goals

   - You can log in to your wordpress administrator page and go to -> Appearance -> Header -> Footer -> Copyright Footer (To add at the bottom of your page)

     similar to this code: 

    html <a rel="license" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank"> <img alt="Creative Commons License" style="border-width:0; margin-right:8px;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" /> </a> <p> © 2025 Almir Cavalcante Filho – This site and its content are licensed under a <a href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank"> Creative Commons Attribution-NonCommercial 4.0 International License </a>. Original work created for educational and portfolio purposes. </p>


   - Will look like this when published:
     
<a rel="license" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">
  <img alt="Creative Commons License" style="border-width:0; margin-right:8px;" 
       src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" />
</a>
<p>
  © 2025 Almir Cavalcante Filho – This site and its content are licensed under a 
  <a href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">
    Creative Commons Attribution-NonCommercial 4.0 International License
  </a>. Original work created for educational and portfolio purposes.
</p>


