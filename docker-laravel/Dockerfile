FROM php:fpm-alpine

#Install dependensi
RUN apk add --no-cache \
    bash \
    curl \
    git \
    openssh-client \
    icu-dev \
    libzip-dev \
    zip \
    unzip

#Install ekstensi PHP
RUN docker-php-ext-install pdo pdo_mysql tokenizer xml curl bcmath json zip

#Copy composer dari lokal ke container (jaringan kantor gak bisa install composer)
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

#Copy project laravel ke image
COPY ./ var/www/app

#Set working directory
WORKDIR /var/www/app

#Set user
USER root

#Memberikan permission untuk folder storage agar tidak error mengakses laravel.log
RUN chown -R root:root storage
RUN chmod -R 777 storage/logs

#Generate key laravel
RUN php artisan key:generate

#Migrate database
RUN php artisan migrate

#Ekspos port container
EXPOSE 80