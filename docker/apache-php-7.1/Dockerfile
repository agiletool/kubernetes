FROM ubuntu:16.04

MAINTAINER robert@trueplus.vn

RUN \
    export LANG=C.UTF-8 && \
    apt-get update && \
    apt-get install -y software-properties-common python-software-properties && \
    add-apt-repository -y -u ppa:ondrej/php && \
    apt-get update && \
    apt-get install -y vim && \
    apt-get install -y imagemagick graphicsmagick && \
    apt-get install -y libapache2-mod-php7.1 php7.1-bcmath php7.1-bz2 php7.1-cli php7.1-common php7.1-curl php7.1-dba php7.1-gd php7.1-gmp php7.1-imap php7.1-intl php7.1-ldap php7.1-mbstring php7.1-mcrypt php7.1-mysql php7.1-odbc php7.1-pgsql php7.1-recode php7.1-snmp php7.1-soap php7.1-sqlite php7.1-tidy php7.1-xml php7.1-xmlrpc php7.1-xsl php7.1-zip && \
    apt-get install -y php-gnupg php-imagick php-mongodb php-streams php-fxsl && \
    sed -i -e 's/max_execution_time = 30/max_execution_time = 300/g' /etc/php/7.1/apache2/php.ini && \
    sed -i -e 's/upload_max_filesize = 2M/upload_max_filesize = 256M/g' /etc/php/7.1/apache2/php.ini && \
    sed -i -e 's/post_max_size = 8M/post_max_size = 512M/g' /etc/php/7.1/apache2/php.ini && \
    sed -i -e 's/memory_limit = 128M/memory_limit = 512M/g' /etc/php/7.1/apache2/php.ini && \
    sed -i -e 's/DirectoryIndex index.html index.php index.cgi index.pl index.xhtml index.htm/DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm/g' /etc/apache2/mods-available/dir.conf && \
    sed -i -r 's/MaxConnectionsPerChild\s+0/MaxConnectionsPerChild   ${MAXCONNECTIONSPERCHILD}/' /etc/apache2/mods-available/* && \
    sed -i -e 's/APACHE_RUN_USER=www-data/APACHE_RUN_USER=ubuntu/g' /etc/apache2/envvars && \
    sed -i -e 's/APACHE_RUN_GROUP=www-data/APACHE_RUN_GROUP=ubuntu/g' /etc/apache2/envvars && \
    echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    echo "<Directory /var/www/html>" >> /etc/apache2/apache2.conf && \
    echo "    Options -Indexes +FollowSymLinks" >> /etc/apache2/apache2.conf && \
    echo "    AllowOverride All" >> /etc/apache2/apache2.conf && \
    echo "    Require all granted" >> /etc/apache2/apache2.conf && \
    echo "</Directory>" >> /etc/apache2/apache2.conf && \
    a2enmod rewrite && \
    rm -rf /usr/src/tmp/ && \
    mkdir /tmp/composer/ && \
    cd /tmp/composer && \
    apt-get install curl && \
    curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer && \
    chmod a+x /usr/local/bin/composer && \
    cd / && \
    rm -rf /tmp/composer && \
    apt-get remove -y python-software-properties software-properties-common && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/* && \
    useradd -d /home/ubuntu -s /bin/bash -m ubuntu && \
    chown -R ubuntu:ubuntu /var/www && \
    chmod 777 -R /var/www

CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
