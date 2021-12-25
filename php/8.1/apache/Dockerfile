FROM php:8.1-apache

RUN usermod -u 48 www-data && groupmod -g 48 www-data
RUN mkdir -p -m 777 /opt/apache/sessiontmp5/

RUN apt update && apt install -y zip curl vim mycli git libzip-dev zlib1g-dev libicu-dev

RUN docker-php-ext-configure intl 
RUN docker-php-ext-install pdo_mysql zip intl

# Install redis
RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis

# Install ZSH
RUN apt install -y zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Symfony cli
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony

COPY docker/php.ini /usr/local/etc/php/conf.d/app.ini
COPY docker/vhost.conf /etc/apache2/sites-available/000-default.conf
COPY docker/apache.conf /etc/apache2/conf-available/z-app.conf

# RUN a2enmod rewrite remoteip && a2enconf z-app

# Clean APT
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# EXPOSE 80
# WORKDIR /var/www/html/
# COPY ./ /var/www/html
# RUN symfony composer install