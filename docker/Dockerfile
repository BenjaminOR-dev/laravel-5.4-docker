FROM php:5.6-apache

# Arguments defined in docker-compose.yml
ARG user
ARG uid

# Install system dependencies
RUN apt-get update && apt-get install -y \
    sudo \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    libmcrypt-dev \
    nano \
    zip \
    unzip \
    nano

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Apache modules
RUN a2enmod rewrite headers

# Install PHP extensions
RUN docker-php-ext-install pdo pdo_mysql mysqli mbstring exif pcntl bcmath gd xml json soap sockets zip

# Copy existing application directory contents
COPY . /var/www/html
WORKDIR /var/www/html

# Git config
RUN git config --global --add safe.directory /var/www/html

# Install composer
COPY --from=composer:2.2 /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER=1

# Site conf file
COPY /docker/site.conf /etc/apache2/sites-available/
RUN a2dissite 000-default && \
    a2ensite site.conf

# Site ini file
COPY /docker/site.ini /usr/local/etc/php/conf.d/

# Restart apache and serve  
RUN service apache2 restart