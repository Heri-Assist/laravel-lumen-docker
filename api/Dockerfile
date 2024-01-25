# Imagen base para PHP 8.3 con composer
FROM php:8.3-apache
RUN echo 'memory_limit = 512M' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini;

RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    libzip-dev\
    zip \
    unzip \
    git \
    libonig-dev \
    libpq-dev \
    zlib1g-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) zip pdo pdo_pgsql bcmath gd sockets

RUN apt-get install -y autoconf pkg-config libssl-dev 

# Establecer la variable de entorno COMPOSER_ALLOW_SUPERUSER
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Establecer la variable de entorno COMPOSER_HOME y actualizar el PATH
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

# Directorio de trabajo
RUN a2enmod rewrite
WORKDIR /var/www/

COPY ./ /var/www/
RUN composer install --no-dev --optimize-autoloader
RUN rm -rf /var/www/html \ 
    && mv /var/www/public /var/www/html \     
    && chmod -R 0777 /var/www/storage/



# Limpiar paquetes y cachés después de la instalación
RUN apt-get purge -y --auto-remove \
&& rm -rf /var/lib/apt/lists/*

# # Entrypoint para iniciar PHP-FPM
# CMD ["php-fpm"]