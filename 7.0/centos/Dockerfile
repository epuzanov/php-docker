FROM centos:7

MAINTAINER Egor Puzanov

RUN mkdir -p /etc/apache2 /var/www/html /usr/lib/cgi-bin && \
    yum -y install centos-release-scl && \
    yum -y install git librsvg2 nmap-ncat httpd24-mod_ssl rh-php70-php \
        rh-php70-php-cli rh-php70-php-fpm rh-php70-php-gd rh-php70-php-intl \
        rh-php70-php-ldap rh-php70-php-mbstring rh-php70-php-mysqlnd \
        rh-php70-php-opcache rh-php70-php-pdo rh-php70-php-pgsql \
        rh-php70-php-xml rh-php70-php-zlib \
        sclo-php70-php-pecl-apcu-bc sclo-php70-php-pecl-memcached unixodbc && \
    yum clean all && \
    rm -rf /tmp/* /var/cache/yum && \
    localedef -i en_US -f UTF-8 C.UTF-8 && \
    ln -s /opt/rh/httpd24/root/etc/httpd/conf.d /etc/apache2/sites-enabled && \
    cd /opt/rh/httpd24/root/etc/httpd/conf.d && \
    rm -rf /var/cache/yum autoindex.conf userdir.conf welcome.conf ssl.conf && \
    ln -sfT /dev/stderr /var/log/httpd24/error_log && \
    ln -sfT /dev/stdout /var/log/httpd24/access_log && \
    sed -i "s/Listen 80/Listen 80\nListen 443/g" /opt/rh/httpd24/root/etc/httpd/conf/httpd.conf && \
    ln -s /opt/rh/httpd24/root/etc/httpd/conf.d/httpd.conf /etc/apache2/apache2.conf && \
    sed -i "s/error_log = .*/error_log = \/proc\/self\/fd\/2/g" /etc/opt/rh/rh-php70/php-fpm.conf && \
    sed -i "s/daemonize = yes/daemonize = no/g" /etc/opt/rh/rh-php70/php-fpm.conf && \
    sed -i "s/user = .*/user = apache/g" /etc/opt/rh/rh-php70/php-fpm.d/www.conf && \
    sed -i "s/group = .*/group = apache/g" /etc/opt/rh/rh-php70/php-fpm.d/www.conf && \
    sed -i "s/listen = .*/listen = 80/g" /etc/opt/rh/rh-php70/php-fpm.d/www.conf && \
    sed -i "s/;clear_env = no/clear_env = no/g" /etc/opt/rh/rh-php70/php-fpm.d/www.conf && \
    sed -i "s/;access.log = .*/access.log = \/proc\/self\/fd\/2/g" /etc/opt/rh/rh-php70/php-fpm.d/www.conf && \
    sed -i "s/;catch_workers_output = yes/catch_workers_output = yes/g" /etc/opt/rh/rh-php70/php-fpm.d/www.conf && \
    ln -s /opt/rh/rh-php70/root/usr/sbin/php-fpm /usr/sbin/php-fpm && \
    ln -s /opt/rh/rh-php70/root/usr/bin/php /usr/bin/php && \
    echo "TLS_REQCERT\tnever" >> /etc/openldap/ldap.conf

COPY httpd-foreground /usr/sbin/httpd-foreground
WORKDIR /var/www/html

EXPOSE 80 443

CMD ["httpd-foreground"]
