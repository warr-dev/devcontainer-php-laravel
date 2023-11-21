# From official php image.
FROM php:8.3.0RC6-zts-alpine3.18
ARG xdebug_enabled=true
# Create a user group and account under id 1000.
RUN addgroup -g 1000 -S user && adduser -u 1000 -D user -G user
# Install quality-of-life packages.
RUN apk add --no-cache bash curl git nano 

# Installing composer
RUN curl -sS https://getcomposer.org/installer -o composer-setup.php
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN rm -rf composer-setup.php

RUN apk update && \
    apk add bash build-base gcc wget git autoconf libmcrypt-dev libzip-dev zip \
    g++ make openssl-dev openssh-client
    
RUN apk add --no-cache nodejs npm

RUN docker-php-ext-install mysqli pdo pdo_mysql && docker-php-ext-enable pdo_mysql
RUN docker-php-ext-install pcntl  

# add cron fo everyminute
RUN mkdir /etc/periodic/1min
COPY ./cron/. /etc/periodic/1min/
RUN chmod -R +x  /etc/periodic/1min
RUN echo "*       *       *       *       *       run-parts /etc/periodic/1min" >> /etc/crontabs/root

# Prevent container from exiting early.
CMD ["sleep", "infinity"]
