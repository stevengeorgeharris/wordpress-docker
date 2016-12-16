#### WordPress local development environment using Docker

This repo provides a solid starting point for local WordPress development with Docker. 

**Prerequisites**
* [Docker](https://www.docker.com/)

New build:
* `git clone ...` 
* `cd wordpress-docker && mkdir site`
* `docker-compose up -d` 

Existing project:
* `git clone ...`
* `cd wordpress-docker && mkdir site`
* clone your WordPress `wp-content` directory into `site/`
* `docker-compose up -d`
* Import your database

**This build contains three images**

Database using the official mariadb image
```
wpdb:
  image: mariadb
  ports:
    - "8081:3306"
  environment:
    MYSQL_ROOT_PASSWORD: root
```

PhpMyAdmin (optional)
```
phpmyadmin:
  image: phpmyadmin/phpmyadmin:latest
  ports:
    - "9090:80"
  links:
    - wpdb:db
  environment:
    MYSQL_USERNAME: root
    MYSQL_ROOT_PASSWORD: root
```

WordPress using the official WordPress image
```
wp: 
  image: wordpress
  volumes:
    - ./site/:/var/www/html
  ports:
    - "8080:80"
  links: 
    - wpdb:mysql
  environment: 
    WORDPRESS_DB_NAME: wordpress
```

An alternative to PhpMyAdmin is Sequel Pro, to connect:

```
Standard Connection

Host: 127.0.0.1
Username: root
Password: root
Post: 8081
```

**TODO::**

* Add shell script
* Import database if available