FROM php:8.3-fpm

ARG USER
ARG UID

# Устанавливаем redis
RUN pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis

# Обновляем систему и ставим необходимые зависимости
RUN apt-get update && apt-get install -y unzip

# Устанавливаем xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Устанавливаем pgsql
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo pdo_pgsql pgsql

# Установливаем intl и gettext для i18n
RUN apt-get install -yqq zlib1g-dev libicu-dev g++ libpq-dev libssl-dev gettext \
    && docker-php-ext-install gettext \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

# Устанавливаем Memcached
RUN apt-get update && apt-get install -y libmemcached-dev \
    && pecl install memcached \
    && docker-php-ext-enable memcached

# Копируем конфиги php
COPY ./php.ini /usr/local/etc/php/php.ini

# Устанавливаем composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/bin --filename=composer

# Добавляем пользователя
RUN useradd -G www-data,root -u $UID -d /home/$USER $USER

# Выставляем рабочую директорию
WORKDIR /var/www/app

# Используем созданного пользователя для создание файлов
USER $USER
