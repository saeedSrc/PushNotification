FROM php:8.1.0-cli

LABEL maintainer="saeed rasooli"

ARG DATA_USER_ID

# Install base packages
RUN apt-get update -y && apt-get install -y && apt-get install git \
        libzip-dev \
        zlib1g-dev \
        libssl-dev \
        libonig-dev \
        supervisor \
        sendmail \
        unzip && \
        docker-php-ext-install -j$(nproc) zip sockets \
        mbstring \
        pdo \
        pdo_mysql \
        zip

# Installing composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/local/bin/composer

RUN docker-php-ext-configure bcmath --enable-bcmath && \
    docker-php-ext-install pcntl bcmath


RUN usermod -u ${DATA_USER_ID} www-data && \
    groupmod -g ${DATA_USER_ID} www-data

# Config apache and copy entrypoint
COPY ./config/* /etc/supervisor/conf.d/
COPY ./entrypoint.sh /

RUN chmod +x /entrypoint.sh

# Override workdir to an existing directory
WORKDIR /var/www

ENTRYPOINT ["/entrypoint.sh"]