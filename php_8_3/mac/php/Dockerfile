FROM php:8.3.4-fpm-alpine3.18

LABEL maintainer="toarupg0318@gmail.com"
LABEL description="PHP 8.3-FPM on Alpine Linux 3.17"

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN apk update && apk add --no-cache \
    supervisor \
    bash \
    git \
    curl-dev \
    zip \
    libzip-dev \
    unzip \
    libpng-dev \
    freetype-dev \
    libjpeg-turbo-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install gd pdo pdo_mysql mysqli

RUN touch /var/run/supervisor.sock
RUN chmod -R 777 /var/run/supervisor.sock

COPY supervisord.conf /etc/supervisord.conf

WORKDIR /var/www/html

COPY docker-php-entrypoint-override /usr/local/bin/docker-php-entrypoint-override
RUN chmod -R 777 /usr/local/bin/docker-php-entrypoint-override
ENTRYPOINT ["/usr/local/bin/docker-php-entrypoint-override"]