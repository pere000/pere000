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

under construction ...

