FROM php:8.1-apache-buster

# Surpresses debconf complaints of trying to install apt packages interactively
# https://github.com/moby/moby/issues/4032#issuecomment-192327844
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get clean

RUN apt-get update && \
    apt-get install -y --no-install-recommends libzip-dev zlib1g-dev libicu-dev libcurl4-openssl-dev

# Update
RUN apt-get -y update --fix-missing && \
    apt-get upgrade -y && \
    apt-get --no-install-recommends install -y apt-utils && \
    rm -rf /var/lib/apt/lists/*

# Install useful tools and install important libraries
RUN apt-get -y update && \
    apt-get -y --no-install-recommends install nano wget dialog \
    libsqlite3-dev libsqlite3-0 default-mysql-client zlib1g-dev libzip-dev \
    libicu-dev build-essential git curl libonig-dev libcurl4 libcurl4-openssl-dev \
    zip openssl && \
    rm -rf /var/lib/apt/lists/* && \
    curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install redis
RUN pecl install redis && \
    docker-php-ext-enable redis

# Install imagick
RUN apt-get update && \
    apt-get -y --no-install-recommends install libmagickwand-dev && \
    rm -rf /var/lib/apt/lists/* && \
    pecl install imagick && \
    docker-php-ext-enable imagick

# Other PHP Extensions
RUN docker-php-ext-install pdo_mysql pdo_sqlite mysqli curl zip intl mbstring gettext exif

# Install Freetype
RUN apt-get -y update && \
    apt-get --no-install-recommends install -y libfreetype6-dev libjpeg62-turbo-dev libpng-dev && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure gd --enable-gd --with-freetype --with-jpeg && \
    docker-php-ext-install gd

# Enable apache modules
RUN a2enmod rewrite headers

# Cleanup
RUN rm -rf /usr/src/*
