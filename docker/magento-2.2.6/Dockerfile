
FROM alpine

MAINTAINER robert@trueplus.vn

ADD . /var/www/html
RUN \
  useradd -d /home/ubuntu -s /bin/bash -m ubuntu && \
  chown -R ubuntu:ubuntu /var/www/html

CMD ["/bin/sh"]
