FROM php:7-fpm

WORKDIR /var/www/api

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && apt-get install -yq --no-install-recommends apt-utils \
    build-essential \
    libxml2-dev \
    libzip-dev \
    libpng-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    libyaml-dev \
    locales \
    bash \
    curl \
    git \
    unzip \
    zip \
    && pecl install redis \
    && pecl install yaml \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd

RUN docker-php-ext-install pdo_mysql zip exif pcntl opcache xml
RUN docker-php-ext-enable redis

RUN docker-php-ext-enable yaml

RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www
RUN chown -R www-data:www-data /var/www

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

USER www

EXPOSE 9000
CMD ["php-fpm"]
