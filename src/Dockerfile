# syntax=docker/dockerfile:1
ARG OLS_VERSION="1.7.16"
ARG PHP_VERSION="74"
ARG PRESTASHOP_VERSION="1.7.8.8"

FROM litespeedtech/openlitespeed:${OLS_VERSION}-lsphp${PHP_VERSION}

ARG PHP_VERSION
ARG PRESTASHOP_VERSION

# Update repositories
RUN apt-get update

# Install some packages
RUN apt-get install -y \
    fontconfig \
    unzip \
    mariadb-client \
    sudo \
    nano

# Install and remove some php extensions
RUN apt-cache --generate pkgnames \
    | grep --line-regexp --fixed-strings \
        -e "lsphp${PHP_VERSION}-common" \
        -e "lsphp${PHP_VERSION}-dev" \
        -e "lsphp${PHP_VERSION}-ioncube" \
        -e "lsphp${PHP_VERSION}-apcu" \
        -e "lsphp${PHP_VERSION}-opcache" \
        -e "lsphp${PHP_VERSION}-memcached" \
        -e "lsphp${PHP_VERSION}-redis" \
        -e "lsphp${PHP_VERSION}-imagick" \
        -e "lsphp${PHP_VERSION}-pear" \
        -e "lsphp${PHP_VERSION}-gd" \
        -e "lsphp${PHP_VERSION}-curl" \
        -e "lsphp${PHP_VERSION}-imap" \
        -e "lsphp${PHP_VERSION}-intl" \
        -e "lsphp${PHP_VERSION}-json" \
        -e "lsphp${PHP_VERSION}-soap" \
        -e "lsphp${PHP_VERSION}-xml" \
        -e "lsphp${PHP_VERSION}-mbstring" \
        -e "lsphp${PHP_VERSION}-mcrypt" \
        -e "lsphp${PHP_VERSION}-pdo" \
        -e "lsphp${PHP_VERSION}-mysql" \
        -e "lsphp${PHP_VERSION}-igbinary" \
        -e "lsphp${PHP_VERSION}-msgpack" \
        -e "lsphp${PHP_VERSION}-pspell-" \
        -e "lsphp${PHP_VERSION}-snmp-" \
        -e "lsphp${PHP_VERSION}-sybase-" \
        -e "lsphp${PHP_VERSION}-tidy-" \
        -e "lsphp${PHP_VERSION}-ldap-" \
        -e "lsphp${PHP_VERSION}-modules-source-" \
    | xargs apt-get install -y \
    && rm -rf /var/lib/apt/lists/*

# Set correct php version as default
RUN rm -f /usr/bin/php \
    && rm -f /usr/local/lsws/fcgi-bin/lsphp \
    && ln -s "/usr/local/lsws/lsphp${PHP_VERSION}/bin/php" /usr/bin/php \
    && ln -s "/usr/local/lsws/lsphp${PHP_VERSION}/bin/lsphp" /usr/local/lsws/fcgi-bin/lsphp

# Fix permissions
RUN chown -R lsadm:lsadm /usr/local/lsws

# Install composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && mv composer.phar /usr/local/bin/composer \
    && php -r "unlink('composer-setup.php');"

# Install phpunit
RUN wget -q https://phar.phpunit.de/phpunit.phar \
    && mv phpunit.phar /usr/local/bin/phpunit \
    && chmod +x /usr/local/bin/phpunit

# Install Prestashop
RUN cd /tmp/ \
    && wget -O prestashop-downloaded.zip "https://github.com/PrestaShop/PrestaShop/releases/download/${PRESTASHOP_VERSION}/prestashop_${PRESTASHOP_VERSION}.zip" \
	&& unzip prestashop-downloaded.zip -d /var/www/vhosts/localhost/html \
	&& rm -rf /tmp/prestashop-downloaded.zip
