FROM docker.io/timoteoramos/alpine:3.18

WORKDIR /var/www/localhost/htdocs

EXPOSE 80

CMD [ "/usr/sbin/httpd", "-DFOREGROUND" ]

RUN apk add composer php81-apache2 && \
    chown -R apache: /var/www/localhost && \
    rm /var/www/localhost/htdocs/index.html && \
    sed -i 's/#LoadModule rewrite_module/LoadModule rewrite_module/g' /etc/apache2/httpd.conf && \
    sed -i 's/AllowOverride [N,n]one/AllowOverride all/g' /etc/apache2/httpd.conf
