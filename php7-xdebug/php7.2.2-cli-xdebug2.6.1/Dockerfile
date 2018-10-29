FROM php:7.2.2-cli as php-packages

# Install developer dependencies
RUN apt-get update -yqq && apt-get install -y \
	libsqlite3-dev \
	libxml2-dev \
	libicu-dev \
	libfreetype6-dev \
	libmcrypt-dev \
	libpng-dev \
	libjpeg62-turbo-dev \
	libcurl4-gnutls-dev \
	libbz2-dev \
	libssl-dev -yqq

# Install php extensions
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install pdo_sqlite
RUN docker-php-ext-install opcache
RUN docker-php-ext-install json
RUN docker-php-ext-install calendar
RUN docker-php-ext-install bcmath
RUN docker-php-ext-install mbstring
RUN docker-php-ext-install sockets
RUN docker-php-ext-install xml
RUN docker-php-ext-install curl
RUN docker-php-ext-install zip
RUN docker-php-ext-install bz2

# Install PECL extensions xdebug
RUN pecl install xdebug-2.6.1
RUN docker-php-ext-enable xdebug

# Install intl
RUN apt-get update \
    && apt-get install -y libicu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl

# Install phar
RUN apt-get update \
    && apt-get install -y libicu-dev \
    && docker-php-ext-configure phar \
    && docker-php-ext-install phar
