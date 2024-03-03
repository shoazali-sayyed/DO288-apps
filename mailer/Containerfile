FROM registry.access.redhat.com/ubi9/ubi:latest

ARG HTTP_PORT=8080

ENV APP_PORT=8080

RUN yum install httpd -y && \
    yum install -y python3 && \
    yum clean all && \
    sed -i -e "s/Listen 80/Listen ${APP_PORT}/g" /etc/httpd/conf/httpd.conf && \
    chgrp -R 0 /var/run/httpd /var/log/httpd && \ 
    chmod -R g=u /var/run/httpd /var/log/httpd 

WORKDIR /var/www/html

COPY index.html .

COPY mailer.py .

LABEL description="An HTTPD Application" \
      io.k8s.description="An Apache application" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="apache, webserver"

EXPOSE ${APP_PORT}

USER 1001

ENTRYPOINT /usr/sbin/httpd -DFOREGROUND
