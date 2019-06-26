FROM tomcat:8.0

LABEL maintainer="saviomathew89@gmail.com"

HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
CMD curl -f http://localhost:8080/companyNews/ || exit 1

COPY companyNews.war /usr/local/tomcat/webapps/

RUN rm -rf /usr/local/tomcat/webapps/ROOT && \
    mv /usr/local/tomcat/webapps/companyNews.war /usr/local/tomcat/webapps/ROOT.war

