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

