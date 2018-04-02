FROM debian:stretch

MAINTAINER Egor Puzanov

ENV DEBIAN_FRONTEND noninteractive

RUN set -x; apt-get update && \
    apt-get install -y --no-install-recommends apache2 apt-transport-https \
        ca-certificates curl git gnupg libapache2-mod-php7.0 librsvg2-bin \
        locales nmap php7.0-cli php7.0-curl php7.0-fpm php7.0-gd php7.0-intl \
        php7.0-ldap php7.0-mbstring php7.0-mysql php7.0-pgsql php7.0-sqlite3 \
        php7.0-xml php7.0-opcache php-apcu-bc php-memcached unixodbc && \
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen && \
    a2enmod macro && \
    a2enmod proxy_fcgi && \
    a2enmod rewrite && \
    a2enmod ssl && \
    a2disconf other-vhosts-access-log && \
    a2dissite 000-default && \
    ln -sfT /dev/stderr /var/log/apache2/error.log && \
    ln -sfT /dev/stdout /var/log/apache2/access.log && \
    ln -sfT /dev/stdout /var/log/apache2/other_vhosts_access.log && \
    echo "TLS_REQCERT never" >> /etc/ldap/ldap.conf && \
    sed -i "s/error_log = .*/error_log = \/proc\/self\/fd\/2/g" /etc/php/7.0/fpm/php-fpm.conf && \
    sed -i "s/;daemonize = yes/daemonize = no/g" /etc/php/7.0/fpm/php-fpm.conf && \
    sed -i "s/user = .*/user = www-data/g" /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i "s/group = .*/group = www-data/g" /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i "s/listen = .*/listen = 80/g" /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i "s/;clear_env = no/clear_env = no/g" /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i "s/;access.log = .*/access.log = \/proc\/self\/fd\/2/g" /etc/php/7.0/fpm/pool.d/www.conf && \
    sed -i "s/;catch_workers_output = yes/catch_workers_output = yes/g" /etc/php/7.0/fpm/pool.d/www.conf && \
    ln -s /usr/sbin/php-fpm7.0 /usr/sbin/php-fpm && \
    mkdir /run/php && \
    rm -fr /var/lib/apt/lists/* /var/cache/apt/archives/* /tmp/*

COPY opcache.ini /etc/php/7.0/mods-available/opcache.ini
COPY httpd-foreground /usr/sbin/httpd-foreground
WORKDIR /var/www/html

EXPOSE 80 443

CMD ["httpd-foreground", "-DFOREGROUND"]
