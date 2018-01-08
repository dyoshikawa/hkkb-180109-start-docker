FROM nginx:mainline-alpine

# add user and setup nginx
RUN adduser -D -s /bin/sh -u 1000 dockeruser \
 && sed -i 's/nginx/dockeruser/g' /etc/nginx/nginx.conf

# set server.conf
ADD server.conf /etc/nginx/conf.d/default.conf

# make wp-dir
RUN mkdir -p /var/www/wp

# set workdir
WORKDIR /etc/nginx