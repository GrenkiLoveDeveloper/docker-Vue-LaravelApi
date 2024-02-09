# Используем образ PHP 8.3 с FastCGI Process Manager (FPM) в качестве базового образа
FROM php:8.3-fpm as build

# Определяем аргументы user и uid, которые можно передать при сборке образа
ARG user
ARG uid

# Устанавливаем системные зависимости и расширения PHP где:

# git \ система контроля версий Git
# curl \ инструмент командной строки для передачи данных
# libzip-dev \ библиотека для работы с ZIP архивами
# libpq-dev \ библиотека для работы с PostgreSQL
# libpng-dev \ библиотека для работы с PNG изображениями
# libonig-dev \ библиотека для работы с регулярными выражениями
# libxml2-dev \ библиотека для работы с XML
# zip \ утилита для работы с ZIP архивами
# unzip \ утилита для распаковки ZIP архивов
# libc-client-dev \ библиотека для работы с почтовыми серверами
# libkrb5-dev \ библиотека для работы с Kerberos
# gnupg2 \ инструмент для шифрования данных
# && apt-get clean && rm -rf /var/lib/apt/lists/* \ команда для очистки кэша и удаления временных файлов
# && pecl install -o -f redis \ установка расширения Redis через PECL
# && docker-php-ext-enable redis \  включение расширения Redis
# && docker-php-ext-install pdo_pgsql pgsql mbstring exif pcntl bcmath gd zip soap \ установка различных расширений PHP
# && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \ конфигурация расширения IMAP с поддержкой Kerberos и SSL
# && docker-php-ext-install imap \ установка расширения IMAP
# && docker-php-ext-enable imap  включение расширения IMAP

RUN apt-get update && apt-get install -y \
    git \
    curl \
    libzip-dev \
    libpq-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    zip \
    unzip \
    libc-client-dev \
    libkrb5-dev \
    gnupg2 \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && pecl install -o -f redis \
    && docker-php-ext-enable redis \
    && docker-php-ext-install pdo_pgsql pgsql mbstring exif pcntl bcmath gd zip soap \
    && docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install imap \
    && docker-php-ext-enable imap

# Копируем установщик расширений PHP из другого образа
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/

# Устанавливаем расширение LDAP
RUN install-php-extensions ldap

# Копируем Composer из другого образа
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
# ENV PATH="$PATH:/root/.composer/vendor/bin"

# Создаем системного пользователя для выполнения команд Composer и Artisan
RUN useradd -G www-data,root -u $uid -d /home/$user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# Копируем кастомный php.ini
COPY php.ini /usr/local/etc/php/php.ini

# Добавляем сертификаты TTK CA (закомментировано)
# COPY ./certs/TTK_Ent_CA.crt /usr/local/share/ca-certificates/
# COPY ./certs/TTK_Root_CA.crt /usr/local/share/ca-certificates/
RUN update-ca-certificates

# Устанавливаем рабочую директорию
WORKDIR /var/www/api

# Устанавливаем пользователя, от имени которого будут выполняться команды
USER $user