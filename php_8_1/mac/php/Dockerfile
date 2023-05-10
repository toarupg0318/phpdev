#FROM php:8.1.1-fpm
FROM php:8.1-fpm-alpine

# enable to use composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN apk add -U --no-cache \
    bash \
    git \
    curl-dev \
    zip \
    libzip-dev \
    unzip \
    supervisor \
    autoconf

RUN apk add --no-cache gmp-dev

RUN docker-php-ext-install pdo\
    pdo_mysql \
    mysqli \
    curl \
    zip \
    bcmath \
    pcntl \
    gmp

RUN apk add -U freetype-dev \
  libjpeg-turbo-dev \
  libpng-dev \
  libwebp-dev

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd

# xdebug
RUN apk --no-cache add \
  autoconf gcc g++ make
RUN apk add --no-cache \
    && pecl install xdebug-3.1.6 \
    && docker-php-ext-enable xdebug
RUN echo "xdebug.mode=debug" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_host=host.docker.internal" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.client_port=9003" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.start_with_request=yes" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini


RUN touch /var/run/supervisor.sock
RUN chmod 777 /var/run/supervisor.sock

COPY docker-php-entrypoint-override /usr/local/bin/docker-php-entrypoint-override
RUN chmod -R 777 /usr/local/bin/docker-php-entrypoint-override
ENTRYPOINT ["/usr/local/bin/docker-php-entrypoint-override"]

#USER 1000:1000
