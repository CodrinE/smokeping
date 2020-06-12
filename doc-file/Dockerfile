FROM alpine:latest
MAINTAINER  eccod


RUN apk add --update smokeping lighttpd perl-cgi curl && rm -rf /var/cache/apk/*

#Create directory Structure
RUN [ -d /usr/data ] || mkdir -p /usr/data && \
    [ -d /usr/cache/smokeping ] || mkdir -p /usr/cache/smokeping


#Setup smokeping directories
RUN mkdir -p /var/www/smokeping/cgi-bin && \
    cp -r /usr/share/webapps/smokeping/* /var/www/smokeping/cgi-bin/ && \
    ln -s /usr/cache/smokeping /var/www/smokeping/cgi-bin/cache

#Change permissions and ownership
RUN chown -R lighttpd:lighttpd /usr/data /usr/cache /var/www/smokeping
RUN chmod -R g+ws /usr/data /usr/cache /var/www/smokeping

# Using prebaked config files since there is a lot of variables to be changed.
ADD config/lighttpd.conf /etc/lighttpd/lighttpd.conf
ADD config/mod_cgi.conf /etc/lighttpd/mod_cgi.conf
ADD config/config /etc/smokeping/config
ADD config/basepage.html /etc/smokeping/basepage.html

ADD config/smokeping.sh /opt/smokeping.sh
RUN chmod 700 /opt/smokeping.sh


EXPOSE 80

ENTRYPOINT /opt/smokeping.sh
