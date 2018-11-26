
FROM alpine

MAINTAINER robert@trueplus.vn

ADD . /var/www/html
RUN \
  adduser -D -h /home/ubuntu -s /bin/sh ubuntu && \
  chown -R ubuntu:ubuntu /var/www/html

CMD ["/bin/sh"]
