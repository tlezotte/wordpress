# wordpress

## How to use this image

### via docker-compose
The easiest way to install these containers is with a docker-compose.yml file. 


```
mkdir wordpress
cd wordpress
curl 
docker-compose up -d
```

#### Example File
```
version: '2'

services:

  wordpress:
    image: tlezotte/ubuntu-wordpress
    ports:
      - 8080:80
      - 8443:443
    environment:
      WORDPRESS_DB_PASSWORD: Q2n0exmL
    volumes:
      - ./html:/var/www/html
      - ./log:/var/log/apache2/

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: Q2n0exmL
    volumes: 
      - ./database:/var/lib/mysql
```

### via docker
```
$ docker run --name some-wordpress --link some-mysql:mysql -d wordpress
```
The following environment variables are also honored for configuring your WordPress instance:

* -e WORDPRESS_DB_HOST=... (defaults to the IP and port of the linked mysql container)
* -e WORDPRESS_DB_USER=... (defaults to "root")
* -e WORDPRESS_DB_PASSWORD=... (defaults to the value of the MYSQL_ROOT_PASSWORD environment variable from the linked mysql container)
* -e WORDPRESS_DB_NAME=... (defaults to "wordpress")
* -e WORDPRESS_TABLE_PREFIX=... (defaults to "", only set this when you need to override the default table prefix in wp-config.php)
* -e WORDPRESS_AUTH_KEY=..., -e WORDPRESS_SECURE_AUTH_KEY=..., -e WORDPRESS_LOGGED_IN_KEY=..., -e WORDPRESS_NONCE_KEY=..., -e WORDPRESS_AUTH_SALT=..., -e WORDPRESS_SECURE_AUTH_SALT=..., -e WORDPRESS_LOGGED_IN_SALT=..., -e WORDPRESS_NONCE_SALT=... (default to unique random SHA1s)

If the __WORDPRESS_DB_NAME__ specified does not already exist on the given MySQL server, it will be created automatically upon startup of the wordpress container, provided that the __WORDPRESS_DB_USER__ specified has the necessary permissions to create it.

If you'd like to be able to access the instance from the host without the container's IP, standard port mappings can be used:

```
$ docker run --name some-wordpress --link some-mysql:mysql -p 8443:443 -d wordpress
```
Then, access it via https://localhost:8443 or https://host-ip:8443 in a browser.

If you'd like to use an external database instead of a linked mysql container, specify the hostname and port with __WORDPRESS_DB_HOST__ along with the password in __WORDPRESS_DB_PASSWORD__ and the username in __WORDPRESS_DB_USER__ (if it is something other than root):

```
$ docker run --name some-wordpress -e WORDPRESS_DB_HOST=10.1.2.3:3306 \
    -e WORDPRESS_DB_USER=... -e WORDPRESS_DB_PASSWORD=... -d wordpress
```