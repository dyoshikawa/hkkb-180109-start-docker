FROM php:7.2.0-fpm-alpine3.7

# install bash
RUN apk --update add bash

# add and set php-fpm dockeruser
RUN adduser -D -s /bin/sh -u 1000 dockeruser \
 && sed -i 's/user\ \=\ www-data/user\ \=\ dockeruser/g' /usr/local/etc/php-fpm.d/www.conf \
 && sed -i 's/group\ \=\ www-data/group\ \=\ dockeruser/g' /usr/local/etc/php-fpm.d/www.conf

# mysql-extention
RUN apk add libjpeg-turbo-dev libpng-dev \
 && docker-php-ext-install mysqli pdo_mysql gd mbstring

# install wordpress
ENV WORDPRESS_TAR_FILE wordpress-latest-ja.tar.gz
RUN curl -O https://ja.wordpress.org/${WORDPRESS_TAR_FILE} \
 && tar -xzf ${WORDPRESS_TAR_FILE} -C /var/www \
 && rm ${WORDPRESS_TAR_FILE}

# set pre-workdir
RUN mv /var/www/wordpress /var/www/wp_pre
WORKDIR /var/www/wp_pre

# add .gitignore
ADD .gitignore .gitignore

# install wordmove
RUN apk add ruby ruby-dev ruby-rdoc ruby-bundler ruby-bigdecimal ruby-irb make gcc build-base \
 && gem install wordmove

# install and setup nodejs-dev-enviroment
RUN apk add nodejs nodejs-npm
ADD package.json package.json
RUN npm install --save-dev webpack babel-cli babel-preset-env babel-loader node-sass \
 && npm install --save bootstrap-sass
ADD .babelrc .babelrc
ADD webpack.config.js webpack.config.js

# set workdir
RUN mkdir /var/www/wp
WORKDIR /var/www/wp

# setup entrypoint
ADD wp-entrypoint.sh /bin/wp-entrypoint.sh
RUN chmod 777 /bin/wp-entrypoint.sh
ENTRYPOINT ["wp-entrypoint.sh"]