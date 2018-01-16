# How To Deploy a Laravel Application with Nginx on Ubuntu 16.04 
Laravel is one of the most popular open-source web application frameworks written in PHP. This framework helps developers build applicationsvby making frequently-used application tasks.

This tutorial, guides you to deploy a simple Laravel application with a production environment in mind, which requires a few common steps. For example, applications should use a dedicated database user with access limited only to necessary databases. File permissions should guarantee that only necessary directories and files are writable. Application settings should be taken into consideration to make sure no debugging information is being displayed to the end user, which could expose application configuration details.

Installing Laravel on Ubuntu 16.04 is pretty much easy. Follow this step by step guide and you should have Laravel up and running in few minutes.

## Step 1 Update and Upgrade
    sudo apt-get update
    sudo apt-get upgrade -y

## Step 2 Create a new sudo user
If you're using a fresh installation of Ubuntu you are proberbaley loggedin as root user. This example creates a new user calledc "Tyrell", but you should replace it with the username you like.
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

## Create mysql database and user
    mysql -u root -p
    CREATE DATABASE laravel DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
    GRANT ALL ON laravel.* TO 'laraveluser'@'localhost' IDENTIFIED BY 'yourpassword';
    FLUSH PRIVILEGES;
    EXIT;
    
## Install PHP
    sudo apt install php-fpm php-mysql php-mbstring php-zip php-xml -y
    
## Configure PHP
    sudo nano /etc/php/7.0/fpm/php.ini
    
    ;cgi.fix_pathinfo=1
    cgi.fix_pathinfo=0
    
## Install the Dependencies
Now, let's install the dependencies. We'll need curl in order to download Composer and php-cli for installing and running it. The php-mbstring package is necessary to provide functions for a library we'll be using. git is used by Composer for downloading project dependencies, and unzip for extracting zipped packages. Everything can be installed with the following command:
                          
    sudo apt-get install curl php-cli php-mbstring git unzip
    
 ## Configure the Nginx server
    sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/example.com
    sudo nano /etc/nginx/sites-available/example.com
    
    root /var/www/laravel/website/public;
    index index.html index.htm index.nginx-debian.html index.php;
    server_name example.com www.example.com;

    try_files $uri $uri/ /index.php?query_string;

    location ~ \.php$ {
    include snippets/fastcgi-php.conf;

    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
    
    sudo ln -s etc/nginx/sites-available/example.com etc/nginx/sites-enabled/example.com
    cd /etc/nginx/sites-available/
    sudo rm default
    sudo service nginx restart
Use the Nginx command to check if your Nginx is configured right.
    
    sudo nginx -t

## Installing en Downloading the Composer
    cd ~
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

Next, run a short PHP script to verify that the installer matches the SHA-384 hash for the latest installer found on the Composer Public Keys / Signatures page. You will need to make sure that you substitute the latest hash for the highlighted value below:
    
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
    
To install composer globally, use the following:
    
    sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
    php -r "unlink('composer-setup.php');"
    sudo apt install composer
    
To test your installation, run:

    composer
    
## Configuring Composer
    sudo composer global require "laravel/installer"
    
## Create Folder to install Laravel into
    cd /var/www
    sudo mkdir example.com
    sudo chown $USER:$USER example.com/
    cd example.com/
    
## Install Laravel Website
    laravel new website
    
## Set permissions and ownership
    sudo chown -R -v $USER:www-data website/
    sudo chmod -R -v 750 website/
    cd website
    sudo chmod -R -v 770 storage/
    sudo chmod -R -v 770 bootstrap/cache
    
## Configure The Enviroment
    cp .env.example .env
    php artisan key:generate
    sudo nano .env

    APP_ENV=production
    APP_DEBUG=false
    APP_KEY=you just generated key
    APP_URL=http://example.com
    DB_HOST=127.0.0.1
    DB_DATABASE=laravel
    DB_USERNAME=laraveluser
    DB_PASSWORD=password

