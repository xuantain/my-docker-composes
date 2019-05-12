# Download base image ubuntu 16.04
FROM ubuntu:16.04
 
# Update Software repository
RUN apt-get update
 
# Install nginx, php-fpm and supervisord from ubuntu repository
RUN apt-get install -y nginx php7.0-fpm supervisor && \
    apt-get install -q -y ssmtp mailutils && \
    rm -rf /var/lib/apt/lists/*
 
# RUN docker-php-ext-install mysql mysqli sysvsem

# Define the ENV variable
ENV nginx_vhost /etc/nginx/sites-available/default
ENV ssmpt_conf /etc/ssmtp/ssmtp.conf
ENV php_sendmail_conf /etc/php/conf.d/php-sendmail.ini
ENV php_conf /etc/php/7.0/fpm/php.ini
ENV xdebug_conf /etc/php/conf.d/xdebug.ini
ENV nginx_conf /etc/nginx/nginx.conf
ENV supervisor_conf /etc/supervisor/supervisord.conf
 
# Enable php-fpm on nginx virtualhost configuration
COPY default ${nginx_vhost}
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' ${php_conf} && \
    echo "\ndaemon off;" >> ${nginx_conf}
 
# Copy supervisor configuration
COPY supervisord.conf ${supervisor_conf}
 
# Config mail service
COPY ssmtp.conf ${ssmpt_conf}
COPY php-sendmail.ini ${php_sendmail_conf}

# RUN echo "hostname=localhost" > /etc/ssmtp/ssmtp.conf \
#     && echo "root=root@localhost" >> /etc/ssmtp/ssmtp.conf \
#     && echo "mailhub=maildev" >> /etc/ssmtp/ssmtp.conf \
    # && echo "sendmail_path=sendmail -i -t" >> /usr/local/etc/php/conf.d/php-sendmail.ini

# RUN echo "[Date]" >> /usr/local/etc/php/conf.d/php-sendmail.ini \
#     && echo "date.timezone = Asia/Hanoi" >> /usr/local/etc/php/conf.d/php-sendmail.ini

# XDebug
COPY xdebug.ini ${xdebug_conf}
# RUN pecl install xdebug-2.5.5 \
#     && echo "zend_extension=$(find /usr/local/lib/php/extensions/ -name xdebug.so)" > /usr/local/etc/php/conf.d/xdebug.ini \
#     && echo "[XDebug]" >> /usr/local/etc/php/conf.d/xdebug.ini \
#     && echo "xdebug.remote_log=/tmp/xdebug.log" >> /usr/local/etc/php/conf.d/xdebug.ini \
#     && echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini \
#     && echo "xdebug.remote_autostart=on" >> /usr/local/etc/php/conf.d/xdebug.ini

# Set local domain
# RUN echo "localhost localhost.localdomain" >> /etc/hosts

RUN mkdir -p /run/php && \
    chown -R www-data:www-data /var/www/html && \
    chown -R www-data:www-data /run/php
 
# Volume configuration
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/conf.d", "/var/log/nginx", "/var/www/html"]
 
# Configure Services and Port
COPY start.sh /start.sh
CMD ["./start.sh"]
 
EXPOSE 80 443

RUN chmod +x start.sh

# BUILD command
# docker build -t nginx_image .
# docker run -d -v /webroot:/var/www/html -p 9000:80 --name app_test nginx_image
