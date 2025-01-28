[![Docker Hub; javieraguerocl/docker-php8.2-with-db-extensions](https://img.shields.io/badge/DOCKER%20HUB-javieraguerocl%2Fdocker--php8.2--with--db--extensions-blue?style=for-the-badge&logo=appveyor)](https://hub.docker.com/r/javieraguerocl/docker-php8.2-with-db-extensions) [![nginx 3.17](https://img.shields.io/badge/nginx-3.17-brightgreen.svg?&logo=nginx&logoColor=white&style=for-the-badge)](https://nginx.org/en/CHANGES) [![php 8.2](https://img.shields.io/badge/php--fpm-8.2-blue.svg?&logo=php&logoColor=white&style=for-the-badge)](https://secure.php.net/releases/7_4_5.php) [![License MIT](https://img.shields.io/badge/license-MIT-blue.svg?&style=for-the-badge)](https://github.com/JavierAgueroCL/docker-php8.2-with-db-extensions/blob/master/Docker/LICENCE)

## Introduction
Docker Compose with Nginx, PHP8.2
Supported DB Extensions: MySQL, MongoDB, SQL Server (MSSQL)
Additional Extensions: GD, Intl, PCNTL, Zip, Exif, WebP, Xdebug, Composer

## Building from source
To build from source you need to clone the git repo and run docker build:
```
$ git clone https://github.com/JavierAgueroCL/docker-php8.2-with-db-extensions.git
$ cd docker-php8.2-with-db-extensions
```

followed by
```
$ docker-compose up
```


## Running on your proyect
Copy the Git files in your Laravel (or something) proyect and run 
```
$ docker-compose up
```

Default web root:
```
/var/www
```

## Build with Docker
```
$ docker build -t javieraguerocl/docker-php8.2-with-db-extensions .
```

## Add your own extensions
You can add your own extensions to the Dockerfile by adding the extension to the Dockerfile and then running the build command again.