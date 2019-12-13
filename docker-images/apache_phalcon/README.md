# docker-images/apache_phalcon/


# apache_phalcon
Is another PHP webserver that supports Phalcon PHP framework.

## Base image:
Debian 9 (Stretch-slim).

## Packages included:
* Apache2 (2.4.25) `$ apache2 -v`
* PHP (7.3.12) `$ php -v`
* Phalcon (3.4.5) `<?php echo Phalcon\Version::get(); ?>`

## PHP extensions:
* php7.3-cli
* php7.3-common
* php7.3-json
* php7.3-opcache
* php7.3-mysql
* php7.3-zip
* php7.3-fpm
* php7.3-mbstring
* php7.3-curl
* php7.3-gd
* php7.3-opcache
* php7.3-sqlite3
* php7.3-tidy
* php7.3-xml
* php7.3-xmlrpc
* php7.3-xsl
* php7.3-pgsql
* php7.3-memcached

If there is anything missing, please let me know to include it in the build.

## Using it:
```
docker run \
       --name phalcon \
       --hostname phalcon \
       --publish 8081:80 \
       -dit deadsoul/apache_phalcon:php-7.3
```

## Link your Database container
```
--link mariadb:db
```
then use `db` as the database host.

## To use your own public_html folder:
```
--volume /path/to/public_html:/var/www/html
```

## Finally
Have fun with coding in Phalcon ;)
