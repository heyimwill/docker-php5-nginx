Just another docker PHP Stack
=================

This is a tuned PHP stack compatible with the Twelve-Factor methodology built on nginx, php-fpm and postfix. It's built with performance in mind and settings have been tweaked to suit production deployments. In accordance to the Twelve-Factor methodology it supports environment variables to connect to a database (postgres, mariadb, etc) by simply linking this container to your database container and using environment variables to define a username/password. It also uses postfix which can be configured to use an external SMTP service (mailgun, mandrill, etc) through environment variables.

You can read more about the Twelve-Factor methodology here: http://12factor.net

## Setup
Grab the latest version of this image from the Docker index:
```
docker pull heyimwill/docker-php5-nginx
```
You can also build the image yourself right from this repo:
```
docker build -t php-stack github.com/heyimwill/docker-php5-nginx
```

## Running
In order for this image to run at all, these environment variables need a defined value: SMTP_HOST, SMTP_USER, SMTP_PASS, DB_USER and DB_PASS. It also needs to be linked with a database container with an alias defined as ```db```.

It expects you to mount your web root to ```/var/www```.

Here's an example of how it can be run:
```
docker run -d --link mariadb:db -v /home/deploy/www:/var/www -e "SMTP_HOST=smtp.mandrillapp.com:587" -e "SMTP_USER=username" -e "SMTP_PASS=pass" -e "DB_USER=username" -e "DB_PASS=pass" php-stack
```

## Database
This container is meant to be run in tandem with a database container, make sure you link this container with a database container assigned the alias ```db```. Here's how it can be used to conigure your app to connect to your database:
```
# Echoes the database hostname
$db_host = $_SERVER["DB_ADDR"];

# Echoes the database port
$db_port = $_SERVER["DB_PORT"];

# Echoes the username
$db_user = $_SERVER["DB_USER"];

# Echoes the password
$db_password = $_SERVER["DB_PASS"];
```

If you don't want to roll your own database container, here's a prebuilt one that you can use: https://github.com/heyimwill/docker-mysql


## Roadmap

