# BUILD: docker build -t deadsoul/apache_phalcon:php-7.3 -f Dockerfile .

FROM debian:stretch-slim

LABEL description="Debian Stretch-Slim image with Apache2, Phalcon & PHP-7.3" \
      maintainer="Mubarak Alrashidi <Mr.DeaDSouL@hotmail.com>" \
      vendor=DeaDSouL \
      name="cafe.unix.images.deb9-phalcon.php-7.3"

RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y locales && rm -rfv /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8

RUN apt-get update && apt-get install -y curl lsb-release apt-transport-https ca-certificates

# PHP & Phalcon repos
RUN curl -s "https://packages.sury.org/php/apt.gpg" -o "/etc/apt/trusted.gpg.d/php.gpg" \
    && echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" > "/etc/apt/sources.list.d/php.list" \
    && curl -s "https://packagecloud.io/install/repositories/phalcon/stable/script.deb.sh" | bash

RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y git php7.3 php7.3-cli php7.3-common php7.3-json php7.3-gd \
    php7.3-mysql php7.3-zip php7.3-fpm php7.3-mbstring php7.3-curl php7.3-opcache \
    php7.3-opcache php7.3-sqlite3 php7.3-tidy php7.3-xml php7.3-xmlrpc php7.3-xsl \
    php7.3-pgsql php7.3-memcached php7.3-apcu php-xdebug php7.3-redis redis-tools \
    php7.3-intl php-psr php7.3-phalcon apache2 libapache2-mod-php7.3

# Enable apache mod rewrite & install phalcon dev-tools
COPY --chown=root:root 000-default.conf /etc/apache2/sites-available/
COPY --chown=root:root composer.json /root/

WORKDIR /root

RUN echo -ne '#!/bin/bash \nservice apache2 start \ntail -f /var/log/apache2/access.log' > /bin/apache2.fg \
    && curl -s http://getcomposer.org/installer | php \
    && service apache2 start && a2enmod rewrite && service apache2 stop \
    && php composer.phar install \
    && chmod ugo+x /root/vendor/phalcon/devtools/phalcon \
    && ln -s /root/vendor/phalcon/devtools/phalcon /usr/local/bin/phalcon \
    && chown -R www-data:www-data /var/www/html

WORKDIR /var/www/html

EXPOSE 80

ENTRYPOINT ["/bin/bash", "/bin/apache2.fg"]
