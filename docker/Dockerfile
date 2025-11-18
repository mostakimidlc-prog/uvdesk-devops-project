FROM php:8.2-apache

# Install dependencies
RUN apt-get update && \
    apt-get install -y git unzip libzip-dev && \
    docker-php-ext-install zip pdo_mysql

# Set working directory
WORKDIR /var/www/uvdesk

# Clone UVdesk Community Skeleton
RUN git clone https://github.com/uvdesk/community-skeleton.git .

# Set permissions
RUN chown -R www-data:www-data /var/www/uvdesk

# Install Composer
RUN php -r "copy('https://getcomposer.org/installer','composer-setup.php');" && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer && \
    rm composer-setup.php

# Install PHP dependencies
RUN composer install --no-dev --optimize-autoloader

# Configure Apache to use public/ as DocumentRoot
RUN sed -i 's|/var/www/html|/var/www/uvdesk/public|g' /etc/apache2/sites-available/000-default.conf

# Enable Apache rewrite module
RUN a2enmod rewrite

EXPOSE 80
