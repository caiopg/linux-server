# Linux Server in Amazon
This README contains the necessary information for linux server hosted in Amazon to be evaluated.

## IP Address
18.217.239.39

## URL to access website
http://18.217.239.39/

## Website
The server is configured to serve the website [tourist-catalog](https://github.com/caiopg/turist-catalog).

## Changes made in server

### Update packages
All packages where updated:
```
sudo apt-get update
sudo apt-get upgrade
```

### Create new user
A new user called `grader` was created:
```
adduser grader
```

### Give new user sudo privileges
Open sudoers file:
```
nano /etc/sudoers
```

Add line to user specification section:
```
grader ALL=(ALL) NOPASSWD:ALL
```

### Create RSA authentication keys
Create keys in local machine:
```
ssh-keygen
```

### Configure RSA keys
Change to grader user:
```
su grader
```

Create .ssh folder and authorized_keys file:
```
mkdir ~/.ssh
nano ~/.ssh/authorized_keys
```

Set adequate permissions:
```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

Paste content of public key in machine inside authorized_keys file in server.

### Configure SSH port
Open SSH file:
```
nano /etc/ssh/ssh_config
```

Change port from 22 to 2200.
Change PasswordAuthentication to `no`.

### Configure UFW
Deny all incoming requests except for SSH(2200), HTTP and NTP:
```
sudo ufw default deny incoming
sudo ufw allow 2200/tcp
sudo ufw allow www
sudo ufw allow ntp
```

Allowing all outgoing requests:
```
sudo ufw default allow outgoing
```

Enable UFW:
```
sudo ufw enable
```

### Configure timezone
Open timezone menu:
```
sudo dpkg-reconfigure tzdata
```

Select option `None of the Above`>`UTC`.

### Install and configure Apache
Install Apache and Python3 wsgi_mod:
```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-wsgi
sudo apt-get install libapache2-mod-wsgi-py3
```

### Install Sqlite
Install SQLite:
```
sudo apt-get install sqlite3 libsqlite3-dev
```

### Install Git
Install Git:
```
sudo apt-get install git
```

### Prepare python
All necessary Python packages were installed.
A `app.wsgi` file was created adding all project folders into Python path and importing Flask app.
Updated database path to absolute path.
Ran `populateDb.py` to create and populate database.

### Configure Apache with wsgi
Opened /etc/apache2/sites-enabled/000-default.conf and added following line:
```
WSGIScriptAlias / /var/www/html/catalog/turist-catalog/app.wsgi
```

### Restart Apache
Restart Apache:
```
sudo /etc/init.d/apache2 restart
```

### Configure Unattended-upgrades packge
Install Unattended-upgrades:
```
sudo apt-get install unattended-upgrades
```

Configure Unattended-upgrades:
```
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

### Disable root SSH access:
Open SSH configuration file:
```
nano /etc/ssh/sshd_config
```

Add following line to file:
```
PermitRootLogin no
```

## Tools used
Amazon
Unattended-upgrades
Apache
Python
SQLite
WSGI
