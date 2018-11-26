
FROM magestore/magento2:php7.1

MAINTAINER robert@trueplus.vn

RUN ln -sf /dev/stdout /var/log/apache2/error.log ; \
    ln -sf /dev/stdout /var/log/apache2/access.log ; \
    ln -sf /dev/stdout /var/log/apache2/other_vhosts_access.log ; \
    rm -rf /var/lib/apt/lists/*

COPY . /var/www/html
RUN chown -R ubuntu:ubuntu /var/www/html

ENTRYPOINT ["/bin/sh", "-c"]
CMD ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
