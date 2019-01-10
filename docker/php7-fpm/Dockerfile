FROM php:7.2-fpm
ARG TIMEZONE

RUN apt-get update && apt-get install -y openssl git unzip zlib1g-dev

COPY xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install pdo pdo_mysql zip bcmath sockets

# install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

RUN echo 'alias sf="/var/www/symfony/bin/console"' >> ~/.bashrc
RUN usermod -u 1000 www-data

# Run this command only once to load data in Docker DB
# RUN php /var/www/symfony/bin/console doctrine:database:import /var/tmp/db_schema.sql

WORKDIR /var/www/symfony
