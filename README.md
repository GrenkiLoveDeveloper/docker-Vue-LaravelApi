# Описание проекта

Scaffolding Docker Laravel + Vue 3 (vite)
Для локальной разработки в докере где фронт и бэк крутятся в одном контейнере
Актуальная версия PHP 8.3

# Как создать проект с нуля самому

## Подробности настройки WSL

1. После перехода в файловую систему WSL в Ubuntu\home\user-name\projects,была создана директория проекта
2. Установка необходимых пакетов для wsl

```
sudo apt update && sudo apt upgrade
sudo apt install curl
sudo apt install wget
sudo apt-get install git
```

3. Установка Node и nvm

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install node
```

4. Устанавливаем php

```
sudo add-apt-repository ppa:ondrej/php
sudo apt update
sudo apt install php8.3 php8.3-cli php8.3-{bz2,curl,mbstring,intl}
sudo apt install php8.3-fpm
```

Если у вас стояла более раняя версия

```
sudo a2disconf php8.2-fpm
sudo apt purge php8.2*
```

5. Проверяем версии

```
php -v
node -v
npm -v
```

6. Устанавливаем composer

```
sudo apt install composer
```

7.  Laravel Installer

```
composer global require laravel/installer
```

После установки вам нужно добавить глобальный каталог Composer в ваш PATH, чтобы вы могли запускать Laravel Installer из любого места.

```
echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >> ~/.bashrc
source ~/.bashrc
```

Проверяем

```
laravel --version
```

## Разворачиваем Laravel

1. Переходим в директорию проекта и устанавливаем laravel в папке api

```
laravel new api
```