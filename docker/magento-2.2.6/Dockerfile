
FROM magestore/magento2:php7.1

MAINTAINER robert@trueplus.vn

COPY . /var/www/html
RUN chmod -R ubuntu:ubuntu /var/www/html

CMD ["/bin/bash"]

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
