# Dockerfile for running php laravel inside apache container

FROM php:7.4.7-apache

RUN apt-get update \
    && apt-get -y upgrade \
    && apt-get install -y libzip-dev zlib1g-dev \
    && apt-get -y autoremove \
    && apt-get install -y libpng-dev\
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip pdo pdo_mysql \
    && docker-php-ext-install gd

EXPOSE 80
EXPOSE 443


ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN a2enmod rewrite headers

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN composer global require laravel/installer \
    && /usr/bin/composer create-project laravel/laravel /var/www/html --prefer-dist \
    && chown www-data:www-data -R /var/www/html/storage /var/www/html/bootstrap/cache

RUN rm -rf /var/www/html/*
COPY . /var/www/html

RUN chown -R www-data:www-data /var/www/html

RUN chmod -R 777 /var/www/html

RUN composer update