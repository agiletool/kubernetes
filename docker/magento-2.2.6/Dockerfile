
FROM alpine

MAINTAINER robert@trueplus.vn

RUN adduser -D -u 1000 ubuntu

ADD . /var/www/html

CMD ["/bin/sh", "-c", "while true; do echo 1; sleep 1; done"]
