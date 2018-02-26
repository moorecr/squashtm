# Pull base image 
From tomcat:8-jre8 

# Maintainer 
MAINTAINER "cmoore@navisite.com" 

ENV TERM='xterm'
ENV SQUASH_TM_VERSION='1.17.0'


# 1.16 version - http://www.squashtest.org/telechargements/send/13-version-stable/265-stm-1160-targz
# 1.17 version - http://www.squashtest.org/telechargements/send/13-version-stable/269-stm-1170-targz
ENV SQUASH_TM_URL='http://www.squashtest.org/telechargements/send/13-version-stable/269-stm-1170-targz'

ENV CATALINA_HOME='/usr/local/tomcat'
ENV JAVA_OPTS="-Xmx1024m -XX:MaxPermSize=256m"


ENV LDAP_ENABLE = 'True'
ENV LDAP_PROVIDER = 'ad-ldap'
ENV LDAP_URL = 'dv01-dc1.syslab.net'
ENV LDAP_SECURITY_MANAGERDN = 'cmoore@syslab.net'
ENV LDAP_SECURITY_MANAGERPASSWORD = 'h3110.hr'
ENV LDAP_FETCH_ATTRIBUTES = True
ENV LDAP_USER_DNPATTERNS = (&(objectclass\=user))
ENV LDAP_USER_SEARCHBASE = 'sAMAccountName'

#ENV DB_TYPE = 'h2'
#ENV DB_URL = 'jdbc:h2:../data/squash-tm'
#ENV DB_USERNAME = 'sa'
#ENV DB_PASSWORD = 'sa'


RUN apt-get -y update && apt-get -y install \
	postgresql-client \
	mysql-client \
	xmlstarlet \
	nano 

RUN mkdir -p /usr/local/tomcat/conf/Catalina/localhost

COPY squash-tm.xml /usr/local/tomcat/conf/Catalina/localhost/squash-tm.xml

RUN cd /usr/share && \
	curl -L ${SQUASH_TM_URL} | gunzip -c | tar x

# Copy WAR to webapps
RUN cp /usr/share/squash-tm/bundles/squash-tm.war ${CATALINA_HOME}/webapps/

COPY docker-entrypoint.sh /usr/share/squash-tm/bin/docker-entrypoint.sh

RUN chmod +x /usr/share/squash-tm/bin/docker-entrypoint.sh

COPY log4j2.xml /usr/share/squash-tm/bin/conf/

EXPOSE 8080

WORKDIR ${CATALINA_HOME}

# Need Entrypoint to be used as a process to start the container
ENTRYPOINT ["/usr/share/squash-tm/bin/docker-entrypoint.sh"]

CMD ["catalina.sh", "run"]





