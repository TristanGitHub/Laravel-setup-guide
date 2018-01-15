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
    
## Install composer
    sudo apt install composer -y

## Configuring Composer
    composer global require "laravel/installer"

