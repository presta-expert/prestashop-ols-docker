# syntax=docker/dockerfile:1
ARG OLS_VERSION="1.7.16"
ARG PHP_VERSION="74"

FROM litespeedtech/openlitespeed:${OLS_VERSION}-lsphp${PHP_VERSION}

ARG PHP_VERSION
ENV PRESTASHOP_VERSION="1.7.8.8"

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
RUN apt-get install -y -m \
	"lsphp${PHP_VERSION}-ioncube" \
    "lsphp${PHP_VERSION}-apcu" \
    "lsphp${PHP_VERSION}-common" \
    "lsphp${PHP_VERSION}-curl" \
    "lsphp${PHP_VERSION}-dev" \
    "lsphp${PHP_VERSION}-igbinary" \
    "lsphp${PHP_VERSION}-imagick" \
    "lsphp${PHP_VERSION}-imap" \
    "lsphp${PHP_VERSION}-intl" \
    "lsphp${PHP_VERSION}-memcached" \
    "lsphp${PHP_VERSION}-msgpack" \
    "lsphp${PHP_VERSION}-mysql" \
    "lsphp${PHP_VERSION}-opcache" \
    "lsphp${PHP_VERSION}-pear" \
    "lsphp${PHP_VERSION}-redis" \
    "lsphp${PHP_VERSION}-json" \
    "lsphp${PHP_VERSION}-pspell-" \
    "lsphp${PHP_VERSION}-snmp-" \
    "lsphp${PHP_VERSION}-sybase-" \
    "lsphp${PHP_VERSION}-tidy-" \
    "lsphp${PHP_VERSION}-ldap-" \
    "lsphp${PHP_VERSION}-modules-source-" \
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
