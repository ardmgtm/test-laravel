FROM php:8.0.2-apache

LABEL MAINTAINER="Wahyu Tri P"

COPY . /var/www/html
COPY .docker/php/vhost.conf /etc/apache2/sites-available/000-default.conf
COPY .docker/php/apache.ini $PHP_INI_DIR/conf.d/

WORKDIR /var/www/html

RUN apt-get update \
    && apt-get install -y --no-install-recommends apt-utils curl gnupg locales apt-transport-https zlib1g-dev libzip-dev unzip libcurl4-openssl-dev pkg-config libssl-dev libxml2-dev

RUN apt-get update \
    && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
    && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
    && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen \
    && apt-get update \
    && ACCEPT_EULA=Y apt-get install -y --no-install-recommends unixodbc-dev msodbcsql17 mssql-tools

RUN \
    apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-configure gd \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get update && apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick \
	&& docker-php-ext-enable imagick

RUN docker-php-ext-install mysqli pdo_mysql zip xml

RUN apt-get update && apt-get install -y apt-transport-https

RUN pecl install sqlsrv pdo_sqlsrv && \
    echo "extension=sqlsrv.so" >> /usr/local/etc/php/conf.d/sqlsrv.ini && \
    echo "extension=pdo_sqlsrv.so" >> /usr/local/etc/php/conf.d/sqlsrv.ini

RUN apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN usermod -u 1000 www-data

RUN a2enmod rewrite && service apache2 restart

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN composer install

RUN apt-get update && apt-get install -y \
    software-properties-common

RUN cp .env.example .env

RUN php artisan key:generate

RUN find /var/www/html -type d -exec chmod 775 {} \;
RUN find /var/www/html -type f -exec chmod 0664 {} \;

RUN chown -R www-data:www-data /var/www/html