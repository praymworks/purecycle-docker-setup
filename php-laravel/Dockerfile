FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Manila

RUN apt-get update && apt-get install -y \
    software-properties-common \
    curl \
    ca-certificates \
    unzip \
    git \
    nano \
    tzdata \
 && add-apt-repository ppa:ondrej/php -y \
 && apt-get update \
 && apt-get install -y \
    php8.2 \
    php8.2-cli \
    php8.2-fpm \
    php8.2-common \
    php8.2-zip \
    php8.2-gd \
    php8.2-xml \
    php8.2-bcmath \
    php8.2-intl \
    php8.2-curl \
    php8.2-mbstring \
    php8.2-mysql \
    php8.2-redis \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Install Composer safely
RUN cd /tmp \
 && curl -sS https://getcomposer.org/installer -o composer-setup.php \
 && HASH="$(curl -sS https://composer.github.io/installer.sig)" \
 && php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('/tmp/composer-setup.php'); exit(1); } echo PHP_EOL;" \
 && php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer \
 && rm -f /tmp/composer-setup.php

WORKDIR /var/www/html

# Optional: tune php-fpm to listen on all interfaces
RUN sed -i 's|^listen = .*|listen = 0.0.0.0:9000|g' /etc/php/8.2/fpm/pool.d/www.conf \
 && sed -i 's|^;clear_env = no|clear_env = no|g' /etc/php/8.2/fpm/pool.d/www.conf

EXPOSE 9000

CMD ["php-fpm8.2", "-F"]