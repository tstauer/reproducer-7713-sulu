FROM php:8.2-fpm

# Install system dependencies & PHP extensions
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        openssl \
        git \
        unzip \
        libzip-dev \
        libmagickwand-dev \
        libicu-dev \
    && docker-php-ext-install -j$(nproc) \
        pdo \
        pdo_mysql \
        opcache \
        zip \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

# Install Imagick
RUN pecl install \
        xdebug-3.2.0 \
        imagick \
    && docker-php-ext-enable \
        xdebug \
        imagick \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer


# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- \
    --install-dir=/usr/local/bin \
    --filename=composer

# Set PHP memory limit to 4GB
RUN echo "memory_limit=4G" > /usr/local/etc/php/conf.d/memory-limit.ini

WORKDIR /var/www/html
