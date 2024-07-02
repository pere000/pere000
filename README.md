# Welcome to My Custom Docker Image: perevictor/nginx-php-bash

Hi there! 👋 This Docker image is derived from the original work by [Tim de Pater](https://hub.docker.com/r/trafex/php-nginx). While Tim de Pater aims to keep his `trafex/php-nginx` as simple as possible, we have extended it to include Bash GNU Shell, adding approximately 300 MB to enable more convenient development of web applications using GNU scripting alongside PHP tasks.

Due to permission settings within the Nginx environment, which I haven't figured out yet, you can't create files, i.e. using touch, echo 'dadada' > filename.txt, etc. You can take a look at the 'Troubleshooting Section' for alternatives.

## Dockerfile Modifications

To integrate additional scripting languages like Python into the App you have to add it to this Dockerfile:

```dockerfile
ARG ALPINE_VERSION=3.19

FROM alpine:${ALPINE_VERSION}

LABEL Maintainer="Your Name <your@email.com>"
LABEL Description="Docker container with Nginx, PHP, and Chrome browser based on Alpine Linux."

# Install necessary packages
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

# Ensure awk points to gawk
RUN ln -sf /usr/bin/gawk /usr/bin/awk

# Configure Nginx, PHP-FPM, and supervisord (as per your application needs)
COPY config/nginx.conf /etc/nginx/nginx.conf
COPY config/fpm-pool.conf /etc/php83/php-fpm.d/www.conf
COPY config/php.ini /etc/php83/conf.d/custom.ini
COPY config/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Make directories and set permissions
RUN mkdir -p /var/www && \
    chown -R nobody:nobody /var/www /run /var/lib/nginx /var/log/nginx

# Create a symbolic link for PHP
RUN ln -s /usr/bin/php83 /usr/bin/php

# Switch to the 'nobody' user for security reasons
USER nobody

# Copy your application code into the container
COPY --chown=nobody src/ /var/www/

# Expose the port used by Nginx
EXPOSE 8080

# Start supervisord to manage Nginx and PHP-FPM
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

# Health check for the container
HEALTHCHECK --timeout=10s CMD curl --silent --fail http://127.0.0.1:8080/fpm-ping || exit 1
.........................................................
#
.........................................................
# Open a command line in the 'www folder' and 'run' your 'Great Tool': 

~/Documents/www$ sudo docker run --name greattool -dp 8080:8080 -v "$(pwd)":/var/www custom-php-bash-nginx

If you place an index.html or index.php file in the 'www' folder, your browser will display its contents at 'http://localhost:8080/'. You can modify the file contents, filenames, delete (rm *), and add (cp * www) files, etc. in any folder, and those changes will be reflected in the browser when you refresh the page. From another perspective, by using the PHP built-in server, instead of a container, your scripts have the potential to access all directories within your file system, enabling you to manage files and services directly from your browser.

In any case, you can use my 'Great Tool': sudo docker pull perevictor/nginx-php-bash:latest

............................... Add Python or any other tool
# Getting Started
Assuming you have Docker installed, follow these steps:

# Build your own Docker Image, with Python if you want:
Run the following command in the directory containing your Dockerfile with the added code to the above Dockerfile (Ask to chatGPT):

    ~/Downloads/docker/docker-php-nginx-master$ sudo docker build -t custom-php-bash-nginx .
    (Note the space and period at the end of the command, they are also code)

# Run Your Container:
Navigate to your project directory (www folder = required) and execute:

    ~/Documents/www$ sudo docker run --name greattool -dp 8080:8080 -v "$(pwd)":/var/www custom-php-nginx

# Access Your Application:
If you have an index.html or index.php file in the www folder, open your browser and go to:

    http://localhost:8080/

To use my Docker image, simply pull it from Docker Hub: sudo docker pull perevictor/nginx-php-bash:latest

## Troubleshouting
When using this Dockerfile, note that due to Nginx configuration, your application may encounter issues creating files using commands like touch or echo 'dadada' > filename. This is typically due to permissions settings within the Nginx environment, which I have not yet resolved.

To avoid this problem, there are two solutions: 1. use perevictor/alpinedrakkar-php-bash_commands0 (39.77 MB) or 2. perevictor/alpinedrakkar-php-mariadb-bash_commands0 (107.29 MB) if you want to make MariaDB available.

This 'sudo docker run --name runtest1 -dp 8080:80 -v "$(pwd)":/var/www perevictor/alpinedrakkar-php-bash_commands0', to run a PHP-CSS-Javascript-Bash_commands 'Locahost Web App', allows your bash scripts to create files and change permissions. Alternatively, using the built-in PHP server instead of a container avoids that problem as well. 

Enjoy developing your web applications with ease using my custom Docker image!
