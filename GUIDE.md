#Docker setup
### Install docker, compose, and machine 

    brew update
    brew install docker docker-compose docker-machine

### Install Virtualbox

[Download Virtualbox](https://www.virtualbox.org/wiki/Downloads)

## Instalation virtual machines with services

### Setup docker environment

**NOTE:** Skip this part for native docker based machine like `ubuntu/debian`.

    docker-machine create -d virtualbox --virtualbox-memory 2048 --virtualbox-disk-size 150000 nginad
    docker-machine start nginad
    docker-machine ssh nginad
        sudo umount /Users && sudo mount -t vboxsf -o uid=33,gid=33 Users /Users
        sudo sh -c "printf '%s\n%s' 'umount /Users' 'mount -t vboxsf -o uid=33,gid=33 Users /Users' >> /var/lib/boot2docker/profile"
        exit
    eval "$(docker-machine env nginad)" OR $(docker-machine env nginad)

### Build & Run

**NOTE:** Use `SUDO` with all `docker`/`docker-compose` commands for native docker based machine like `ubuntu/debian`

    docker-compose build
    docker-compose up -d

Your docker machine IP can be found via (native docker based system 127.0.0.1)

    docker-machine ls
    
To Check all the container running use below:
    
    docker-compose ps

Add this ip to your `hosts` file

    sudo echo '{IP_ADDRESS}  nginad.local' >> /etc/hosts


#How to deploy on local

## Composer Installing
Run: composer install
or: php composer.phar install

## Configuration
1. Copy .env.example to .env
2. Edit file .env to config your database

## Migration
1. php artisan vendor:publish --force --tag=migrations 
2. php artisan migrate

## Install theme
Run: /usr/local/bin/install

## NOTES
* Middleware handles permission checking User/Middleware/Authorize.php
