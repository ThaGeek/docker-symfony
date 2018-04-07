# Docker Symfony (PHP7 - NGINX - MySQL)

Docker-symfony gives you everything you need for developing Symfony application. This complete stack run with docker and [docker-compose (1.7 or higher)](https://docs.docker.com/compose/).

## Installation

1. Create a `.env` from the `.env.dist` file. Adapt it according to your symfony application

    ```bash
    cp .env.dist .env
    ```


2. Build/run containers with (with and without detached mode)

    ```bash
    $ docker-compose build
    $ docker-compose up -d
    ```

3. Update your system host file (add symfony.local)

    ```bash
    # UNIX only: get containers Gateway IP address and update host (replace IP according to your configuration) (on Windows, edit C:\Windows\System32\drivers\etc\hosts)
    $ echo $(sudo docker network inspect bridge | grep Gateway | grep -o -E '[0-9\.]+') # Will output the Gateway IP
    ```
    Add a row in /etc/hosts with the following information : 
    [GATEWAY_IP] symfony.local

    
4. Prepare Symfony app

    ```bash
    $ docker-compose exec php bash
    $ composer install
    $ sf3 doctrine:database:create --env=prod
    $ sf3 cache:clear --env=prod
    $ chmod -R 777 var/cache var/logs var/sessions
    $ php bin/console doctrine:migrations:migrate --env=prod
    ```

5. Enjoy :-)


## Useful commands

```bash
# bash commands
$ docker-compose exec php bash

# Composer (e.g. composer update)
$ docker-compose exec php composer update

# SF commands (Tips: there is an alias inside php container)
$ docker-compose exec php php /var/www/symfony/bin/console cache:clear # Symfony3
# Same command by using alias
$ docker-compose exec php bash
$ sf cache:clear

# Retrieve an IP Address (here for the nginx container)
$ docker inspect --format '{{ .NetworkSettings.Networks.dockersymfony_default.IPAddress }}' $(docker ps -f name=nginx -q)
$ docker inspect $(docker ps -f name=nginx -q) | grep IPAddress

# MySQL commands
$ docker-compose exec db mysql -uroot -p"root"

# Giving full permissions to acces cache/logs folder
$ sudo chmod -R 777 var/cache var/logs var/sessions # Symfony3

```