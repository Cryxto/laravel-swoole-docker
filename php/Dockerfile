
# FROM php:8.2-fpm

FROM php:8.2-fpm-alpine

ENV PS1="\u@\h:\w\\$ "
ENV TZ="Asia/Jakarta"

ENV COMPOSER_MEMORY_LIMIT='-1'

# RUN apt-get update && \
#     apt-get install -y --force-yes --no-install-recommends \
#         libmemcached-dev \
#         libmcrypt-dev \
#         libreadline-dev \
#         libgmp-dev \
#         libzip-dev \
#         libz-dev \
#         libpq-dev \
#         libjpeg-dev \
#         libpng-dev \
#         libfreetype6-dev \
#         libssl-dev \
#         openssh-server \
#         libmagickwand-dev \
#         git \
#         cron \
#         nano \
#         libxml2-dev
RUN apk update && \
    apk add --no-cache \
        linux-headers \
        libmemcached-dev \
        libmcrypt-dev \
        readline-dev \
        gmp-dev \
        libzip-dev \
        zlib-dev \
        postgresql-dev \
        jpeg-dev \
        libpng-dev \
        freetype-dev \
        openssl-dev \
        openssh \
        imagemagick-dev \
        git \
        dcron \
        nano \
        libxml2-dev \
        autoconf \
        gcc \
        g++ \
        make \
        nodejs \ 
        npm

# Install soap extention
RUN docker-php-ext-install soap

# Install for image manipulationdock
RUN docker-php-ext-install exif

# Install the PHP pcntl extention
RUN docker-php-ext-install pcntl

# Install the PHP intl extention
RUN docker-php-ext-install intl

# Install the PHP gmp extention
RUN docker-php-ext-install gmp

# Install the PHP zip extention
RUN docker-php-ext-install zip

# Install the PHP pdo_mysql extention
RUN docker-php-ext-install pdo_mysql

# Install the PHP pdo_pgsql extention
RUN docker-php-ext-install pdo_pgsql

# Install the PHP bcmath extension
RUN docker-php-ext-install bcmath

#####################################
# PHPRedis:
#####################################
RUN pecl install redis && docker-php-ext-enable redis
RUN pecl install mongodb && docker-php-ext-enable mongodb
#####################################
# Imagick:
#####################################

RUN pecl install imagick && \
    docker-php-ext-enable imagick


#####################################
# Swoole:
#####################################
RUN pecl install swoole && \
    docker-php-ext-enable swoole

#####################################
# apcu:
#####################################
RUN pecl install apcu && \
    docker-php-ext-enable apcu
RUN echo "extension=apcu.so" >> /etc/php.ini
RUN echo "apc.enable_cli=1" >> /etc/php.ini


#####################################
# APFD:
# Install APFD for multipart for support
#####################################
RUN pecl install apfd && \
    docker-php-ext-enable apfd


#####################################
# GD:
#####################################

# Install the PHP gd library
RUN docker-php-ext-install gd && \
#    docker-php-ext-configure gd \
#        --with-jpeg-dir
#        --with-freetype-dir=/usr/include/freetype2 && \
     docker-php-ext-configure gd --with-freetype --with-jpeg && \
     docker-php-ext-install gd
#####################################
# xDebug:
#####################################

# Install the xdebug extension
RUN pecl install xdebug

#####################################
# PHP Memcached:
#####################################

# Install the php memcached extension
RUN pecl install memcached && docker-php-ext-enable memcached

#####################################
# Composer:
#####################################

# Install composer and add its bin to the PATH.
RUN curl -s http://getcomposer.org/installer | php && \
    echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc && \
    mv composer.phar /usr/local/bin/composer
RUN echo 'export COMPOSER_ALLOW_SUPERUSER=1' >> ~/.bashrc
# Source the bash
RUN . ~/.bashrc
RUN source ~/.bashrc

#####################################
# Laravel Schedule Cron Job:
#####################################

RUN echo "* * * * * root /usr/local/bin/php /var/www/artisan schedule:run >> /dev/null 2>&1"  >> /etc/cron.d/laravel-scheduler
RUN chmod 0644 /etc/cron.d/laravel-scheduler

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#

ADD ./local.ini /usr/local/etc/php/conf.d

#####################################
# Aliases:
#####################################

# # docker-compose exec php-fpm art --> php artisan
RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/html/artisan "$@"' > /usr/bin/artisan
RUN chmod +x /usr/bin/artisan
RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/html/artisan octane:start "$@"' > /usr/bin/octane
RUN chmod +x /usr/bin/octane

# # docker-compose exec php-fpm dep --> locally installed Deployer binaries
# RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/vendor/bin/dep "$@"' > /usr/bin/dep
# RUN chmod +x /usr/bin/dep
# # docker-compose exec php-fpm art --> php artisan
# RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan "$@"' > /usr/bin/art
# RUN chmod +x /usr/bin/art
# # docker-compose exec php-fpm migrate --> php artisan migrate
# RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan migrate "$@"' > /usr/bin/migrate
# RUN chmod +x /usr/bin/migrate
# # docker-compose exec php-fpm fresh --> php artisan migrate:fresh --seed
# RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan migrate:fresh --seed' > /usr/bin/fresh
# RUN chmod +x /usr/bin/fresh
# # docker-compose exec php-fpm t --> run the tests for the project and generate testdox
# RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan config:clear\n/var/www/vendor/bin/phpunit -d memory_limit=2G --stop-on-error --stop-on-failure --testdox-text=tests/report.txt "$@"' > /usr/bin/t
# RUN chmod +x /usr/bin/t
# # docker-compose exec php-fpm d --> run the Laravel Dusk browser tests for the project
# RUN echo '#!/bin/bash\n/usr/local/bin/php /var/www/artisan config:clear\n/bin/bash\n/usr/local/bin/php /var/www/artisan dusk -d memory_limit=2G --stop-on-error --stop-on-failure --testdox-text=tests/report-dusk.txt "$@"' > /usr/bin/d
# RUN chmod +x /usr/bin/d

# RUN rm -r /var/lib/apt/lists/*
ENV COMPOSER_ALLOW_SUPERUSER 1


WORKDIR /var/www

RUN chown -R www-data:www-data /var/www


COPY ./docker-entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-entrypoint.sh
RUN ln -s /usr/local/bin/docker-entrypoint.sh /

# RUN composer install

# ENTRYPOINT ["docker-entrypoint.sh"]

# EXPOSE 9000
EXPOSE 8000
# RUN composer require laravel/octane
# RUN npm install --save-dev chokidar
ARG USER_UID=1000
ARG GROUP_GID=1000
ARG UGNAME=cryxto

RUN if getent passwd ${USER_UID} >/dev/null; then \
    deluser $(getent passwd ${USER_UID} | cut -d: -f1); fi

RUN if getent group ${GROUP_GID} >/dev/null; then \
    delgroup $(getent group ${GROUP_GID} | cut -d: -f1); fi

RUN addgroup --system --gid ${GROUP_GID} ${UGNAME}

RUN adduser --system --disabled-password --home /home/${UGNAME} \
    --uid ${USER_UID} --ingroup ${UGNAME} ${UGNAME}
# RUN useradd -u 1000 -g 1000 cryxto
# USER cryxto
USER ${UGNAME}

CMD composer install && composer require laravel/octane && php artisan octane:install --server=swoole && npm install --save-dev chokidar && php artisan octane:start --host=0.0.0.0 --watch
# CMD composer install && php artisan octane:start --host=0.0.0.0 --max-requests=1000