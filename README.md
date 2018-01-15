# Laravel-setup-guide
Laravel setup guide for Ubuntu 16.04 on DigitalOcean

## Update The Servers Definitions and Software
    sudo apt-get update
    sudo apt-get upgrade -y

## Create new sudo user
    adduser tyrell
    usermod -aG sudo tyrell
    su - tyrell
    
## Create/Add SSH key for logging in
    mkdir ~/.ssh
    chmod 700 ~/.ssh
    nano ~/.ssh/authorized_keys
    chmod 600 ~/.ssh/authorized_keys
    
## Update The Servers Definitions and Software
    sudo apt-get update
    sudo apt-get upgrade -y
    
## Install nginx
    sudo apt install nginx -y
    
## Install mysql server
    sudo apt install mysql-server -y
    
## Install PHP
    sudo apt install php-fpm php-mysql php-mbstring php-zip php-xml -y
    
## Configure PHP
    sudo nano /etc/php/7.0/fpm/php.ini
    
    ;cgi.fix_pathinfo=1
    cgi.fix_pathinfo=0
    
## Install the Dependencies
Now, let's install the dependencies. We'll need curl in order to download Composer and php-cli for installing and running it. The php-mbstring package is necessary to provide functions for a library we'll be using. git is used by Composer for downloading project dependencies, and unzip for extracting zipped packages. Everything can be installed with the following command:
                          
    sudo apt-get install curl php-cli php-mbstring git unzip

## Installing en Downloading the Composer
    cd ~
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

Next, run a short PHP script to verify that the installer matches the SHA-384 hash for the latest installer found on the Composer Public Keys / Signatures page. You will need to make sure that you substitute the latest hash for the highlighted value below:
    
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    
To install composer globally, use the following:
    
    php composer-setup.php
    php -r "unlink('composer-setup.php');"
    
To test your installation, run:

    composer

## Configuring Composer
    composer global require "laravel/installer"

