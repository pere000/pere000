### Hi there 👋

Fork based on the original work from "Tim de Pater <code@trafex.nl>" at https://hub.docker.com/r/trafex/php-nginx.

Although the porpouse of Mr. Tim de Pater is to keep his original trafex/php-nginx 'as simple as possible', we have added 'Bash GNU Shell' to his 
original hub.docker.com/r/trafex/php-nginx By adding about 6 Mb for Bash, you get convenience in development to code web applications that use advanced 
bash scripts—instead of Python, Perl, etc.—to implement PHP tasks. Although you can work with these languages ​​from the localhost command line 
by temporarily changing the owner of the files to work with.

We have changed the WORKDIR from /var/www/html to /var/www in the line 7, added the 'Bash GNU Shell' at line 14 and, in consecuence, we changed lines 48 
and 52 of the derivative Dockerfile. We have also changed the nginx.conf file (line 42 of the derivative) for accessing to any files in the web root folder, 
and the availability of 'txt' files by gzip_type on line 90. For security reasons, these changes are not designed for running on Internet domains but only 
on the localhost domain (127.1.0.0.1).

Any security weaknesses are due to the built-in native kernels (nginx, PHP, Bash, etc.), browser engine, and operating system security on the user side. But 
this traffex/php-nginx derivative (fork) in 'expert hands' should not present additional security risks on the Internet network. Either way, you can always 
create your own derivative with Mr. Tim Pater's original at https://github.com/TrafeX/docker-php-nginx/archive/refs/heads/master.zip There you can modify 
your Dockerfile, the corresponding codes in 'nginx.conf', and other files.

Let us suppose you have run 'sudo docker pull perevictor/fffwebapp' and have succeeded. After also running: 
sudo docker run --name=ArbitraryTag -dp 80:8080 fffwebapp:latest you can access the server at http://localhost/index.html. 
The index.html page will show you how to use fffwebapp:latest to work with any directory containing an HTML-PHP-Javascript-Bash application for 
localhost (127.0.0.1).

