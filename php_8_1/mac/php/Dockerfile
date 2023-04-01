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
    supervisor

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

RUN touch /var/run/supervisor.sock
RUN chmod 777 /var/run/supervisor.sock

COPY docker-php-entrypoint-override /usr/local/bin/docker-php-entrypoint-override
RUN chmod -R 777 /usr/local/bin/docker-php-entrypoint-override
ENTRYPOINT ["/usr/local/bin/docker-php-entrypoint-override"]

#USER 1000:1000
