# wordpress

## Install with docker-compose
The easiest way to install these containers is with a docker-compose.yml file. The compose file will download and start two containers. The first container will start a customized version of the Official Wordpress container. The second is an Official MariaDB container.

The customized Wordpress container has the following added features.
* https enabled using a self-signed certificate
* [wp-cli](http://wp-cli.org/)
  * A script (__wp__) that will run __wp-cli__, while logged in as root

Docker compose will mount the following directories where the command is run.
* database = MariaDB data files
* html = Web root
* database_log = Log files for MariaDB
* html_log = Log files for Apache

### How to run docker compose
```
mkdir wordpress
cd wordpress
curl -O https://raw.githubusercontent.com/tlezotte/wordpress/master/docker-compose.yml
** read note below **
docker compose up -d
```
wait for it to initialize completely, and visit [https://localhost:8443](https://localhost:8443).

__NOTE:__ Change the passwords for __WORDPRESS_DB_PASSWORD__ and __MYSQL_ROOT_PASSWORD__ before running `docker compose up -d`. The passwords need to be the same.

### How to connect to container
#### list running containers
```
docker ps -a
```
#### control containers
```
docker stop <container id>
docker start <container id>
docker restart <container id>
```
#### connect to container
```
docker exec -it <container id> bash
docker exec -it <image name> bash
```

#### Example File
```
version: '2'

services:

  wordpress:
    image: tlezotte/wordpress
    ports:
      - 8443:443
    environment:
      WORDPRESS_DB_PASSWORD: Q2n0exmL
    volumes:
      - ./html:/var/www/html
      - ./html_log:/var/log/apache2/

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: Q2n0exmL
    volumes: 
      - ./database:/var/lib/mysql
      - ./database_log:/var/log/mysql
```
