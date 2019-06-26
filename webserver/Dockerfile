FROM centos:7

MAINTAINER saviomathew89@gmail.com

RUN yum -y install httpd  mod_ssl && \
	mkdir /etc/httpd/ssl && \
	rm -rf /etc/httpd/conf.d/welcome.conf


COPY static /var/www/
COPY www.companynews.com.conf /etc/httpd/conf.d/
COPY ssl/* /etc/httpd/ssl/

EXPOSE 80

ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
