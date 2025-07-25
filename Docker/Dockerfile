FROM php:8.2-fpm

LABEL maintainer="Javier Aguero javieraguerocl@gmail.com"

ENV ACCEPT_EULA=Y

ARG DEBIAN_FRONTEND=noninteractive

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libwebp-dev \
    locales \
    zip \
    libzip-dev \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl gnupg gnupg2 gnupg1

# Install apt utils
RUN apt-get install -y apt-utils

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install MS ODBC Driver for SQL Server
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/11/prod.list > /etc/apt/sources.list.d/mssql-release.list 
RUN apt-get update 
RUN apt-get -y --no-install-recommends install msodbcsql17
# Optionals
RUN apt-get install -y mssql-tools
RUN apt-get install -y libgssapi-krb5-2
RUN apt-get -y --no-install-recommends install unixodbc-dev 
RUN apt-get install -y nano
RUN apt-get install -y libssl-dev pkg-config

#  End Optionals
RUN pecl install sqlsrv 
RUN pecl install pdo_sqlsrv 
RUN echo "extension=pdo_sqlsrv.so" >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-pdo_sqlsrv.ini 
RUN echo "extension=sqlsrv.so" >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/30-sqlsrv.ini 

# OpenSSL for MSSQL Remote Connection Support
RUN apt-get install -y --no-install-recommends openssl \ 
    && sed -i 's,^\(MinProtocol[ ]*=\).*,\1'TLSv1.0',g' /etc/ssl/openssl.cnf \
    && sed -i 's,^\(CipherString[ ]*=\).*,\1'DEFAULT@SECLEVEL=1',g' /etc/ssl/openssl.cnf\
    && rm -rf /var/lib/apt/lists/*
RUN apt-get clean; rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Copy OpenSSL CFG
COPY ./php/openssl.cnf /etc/ssl/openssl.cnf

# Install extensions
# RUN docker-php-ext-install mysql
RUN docker-php-ext-install pdo_mysql zip exif pcntl mysqli

#GD Extension with FreeType, JPEG, WebP
RUN docker-php-ext-configure gd \
    --with-freetype \
    --with-jpeg \
    --with-webp \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get -y update \
    && apt-get install -y libicu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

#Soap Extension
RUN apt-get update && \
    apt-get install -y libxml2-dev
RUN docker-php-ext-install soap


#MongoDB Extension Install
RUN pecl install mongodb \
    &&  echo "extension=mongodb.so" > $PHP_INI_DIR/conf.d/mongo.ini 

#Xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug
RUN mkdir -p docker/php/conf.d
COPY ./php/xdebug.ini /docker/php/conf.d/xdebug.ini
RUN touch docker/php/conf.d/error_reporting.ini

# Install composer 1
# RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \ && composer self-update --1

# Install composer 2
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

# Install WP-CLI
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

# Install SVN
RUN apt-get install -y subversion

# Install phpunit
RUN curl -LsS https://phar.phpunit.de/phpunit-10.phar > /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit

# Install mysqladmin
RUN apt-get install -y default-mysql-client

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
