FROM centos:8

MAINTAINER Egor Puzanov

RUN mkdir -p /etc/apache2 /var/www/html /usr/lib/cgi-bin && \
    dnf -y install git librsvg2 nmap-ncat mod_ssl php \
        php-cli php-fpm php-gd php-intl php-ldap php-mbstring php-mysqlnd \
        php-opcache php-pdo php-pgsql php-xml php-zlib php-pecl-apcu && \
    dnf clean all && \
    cd /etc/httpd/conf.d && \
    rm -rf /tmp/* /var/cache/dnf autoindex.conf userdir.conf welcome.conf ssl.conf && \
    ln -sfT /dev/stderr /var/log/httpd/error_log && \
    ln -sfT /dev/stdout /var/log/httpd/access_log && \
    sed -i "s/Listen 80/Listen 80\nListen 443/g" /etc/httpd/conf/httpd.conf && \
    ln -s /etc/httpd/conf/httpd.conf /etc/apache2/apache2.conf && \
    sed -i "s/error_log = .*/error_log = \/proc\/self\/fd\/2/g" /etc/php-fpm.conf && \
    sed -i "s/daemonize = yes/daemonize = no/g" /etc/php-fpm.conf && \
    sed -i "s/user = .*/user = apache/g" /etc/php-fpm.d/www.conf && \
    sed -i "s/group = .*/group = apache/g" /etc/php-fpm.d/www.conf && \
    sed -i "s/listen = .*/listen = 80/g" /etc/php-fpm.d/www.conf && \
    sed -i "s/;clear_env = no/clear_env = no/g" /etc/php-fpm.d/www.conf && \
    sed -i "s/;access.log = .*/access.log = \/proc\/self\/fd\/2/g" /etc/php-fpm.d/www.conf && \
    sed -i "s/;catch_workers_output = yes/catch_workers_output = yes/g" /etc/php-fpm.d/www.conf && \
    echo "TLS_REQCERT\tnever" >> /etc/openldap/ldap.conf

COPY httpd-foreground /usr/sbin/httpd-foreground
WORKDIR /var/www/html

EXPOSE 80 443

CMD ["httpd-foreground"]
