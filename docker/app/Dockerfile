FROM php:7.4.9-fpm
COPY php.ini /usr/local/etc/php/

RUN apt update \
  && apt install -y wget git unzip libpq-dev zlib1g-dev mariadb-client libzip-dev \
  && docker-php-ext-install zip pdo_mysql

#XDebug install
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

#Node.js install
RUN apt install -y gnupg
RUN curl -sL https://deb.nodesource.com/setup_13.x | bash -
RUN apt install -y nodejs

#Composer install バージョン1指定
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('sha384', 'composer-setup.php') === file_get_contents('https://composer.github.io/installer.sig')) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php --version=2.0.8
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_MEMORY_LIMIT -1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www

RUN composer global require "laravel/installer"