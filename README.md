### Hi there 👋

Fork based on the original work from "Tim de Pater <code@trafex.nl>" at https://hub.docker.com/r/trafex/php-nginx.

Although the porpouse of Mr. Tim de Pater is to keep his original trafex/php-nginx 'as simple as possible', we have added 'Bash GNU Shell' to his 
original hub.docker.com/r/trafex/php-nginx By adding 300 Mb more for Bash, you gain development convenience for coding web applications that use GNU scripting intensively to implement PHP tasks. Ask chatGPT how to change the Dockerfile to add other scripting languages, for example Python.

We shortened the WORKDIR from /var/www/html to /var/www, and added 
    bash \
    coreutils \
    findutils \
    grep \
    gawk \
    sed \
    ttf-freefont
    
You only need to download the original from 'Tim de Pater': https://github.com/TrafeX/docker-php-nginx. Then change its Dockerfile by the following: 

.............................................................

ARG ALPINE_VERSION=3.19

FROM alpine:${ALPINE_VERSION}

LABEL Maintainer="Your Name <your@email.com>"

LABEL Description="Docker container with Nginx, PHP, and Chrome browser based on Alpine Linux."

##Install necessary packages
RUN apk add --no-cache \
    curl \
    nginx \
    php83 \
    php83-ctype \
    php83-curl \
    php83-dom \
    php83-fileinfo \
    php83-fpm \
    php83-gd \
    php83-intl \
    php83-mbstring \
    php83-mysqli \
    php83-opcache \
    php83-openssl \
    php83-phar \
    php83-session \
    php83-tokenizer \
    php83-xml \
    php83-xmlreader \
    php83-xmlwriter \
    supervisor \
    bash \
    coreutils \
    findutils \
    grep \
    gawk \
    sed \
    ttf-freefont

##Configure Nginx, PHP-FPM, and supervisord (as per your application needs)

COPY config/nginx.conf /etc/nginx/nginx.conf

COPY config/fpm-pool.conf /etc/php83/php-fpm.d/www.conf

COPY config/php.ini /etc/php83/conf.d/custom.ini

COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

##Make directories and set permissions

RUN mkdir -p /var/www && \

chown -R nobody:nobody /var/www /run /var/lib/nginx /var/log/nginx

##Create a symbolic link for PHP

RUN ln -s /usr/bin/php83 /usr/bin/php

##Switch to the 'nobody' user for security reasons

USER nobody

##Copy your application code into the container

COPY --chown=nobody src/ /var/www/

##Expose the port used by Nginx

EXPOSE 8080

##Start supervisord to manage Nginx and PHP-FPM

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

##Health check for the container

HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping || exit 1


..................................................................


Assuming you have Docker installed, run 'build': 

~/Downloads/docker/docker-php-nginx-master$ sudo docker build -t custom-php-nginx1 . 

Notice that the 'space and dot' at the end of the above command is 'more code'. Now that you have your 'Great Tool', select some folder of HTML-CSS-Javascript-PHP-GNU command scripts placed, inside a 'www folder', in your Desktop file system. Open a command line in the 'www folder' and 'run' your 'Great Tool': 

~/Documents/www$ sudo docker run --name greattool -dp 8080:8080 -v "$(pwd)":/var/www custom-php-nginx1

If you place an index.html or index.php file in the 'www' folder, your browser will display its contents at 'http://localhost:8080/'. You can modify the file contents, filenames, delete (rm *), and add (cp * www) files, etc. in any folder, and those changes will be reflected in the browser when you refresh the page. From another perspective, by using the PHP built-in server, instead of a container, your scripts have the potential to access all directories within your file system, enabling you to manage files and services directly from your browser.

In any case, you can use my 'Great Tool': sudo docker pull perevictor/nginx-php-bash:latest

