FROM nginx:mainline-alpine

RUN adduser -D -s /bin/sh -u 1000 dockeruser \
 && sed -i 's/nginx/dockeruser/g' /etc/nginx/nginx.conf

ADD server.conf /etc/nginx/conf.d/default.conf

RUN mkdir -p /var/www/wp

WORKDIR /etc/nginx