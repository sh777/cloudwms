FROM ubuntu:14.04
MAINTAINER YiFeng Huang "withoutbug@gmail.com"

ENV TOMCAT_DOWNLOAD_URL http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.0.41/bin/apache-tomcat-8.0.41.tar.gz
ENV CATALINA_HOME /opt/tomcat

RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections

# Install Oracle JDK 7
RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common && \
    add-apt-repository ppa:webupd8team/java && \
    apt-get update && \
    apt-get install -y --no-install-recommends oracle-java7-installer && \
    rm -rf /var/lib/apt/lists/*
    
# Install Tomcat 8
RUN wget -q ${TOMCAT_DOWNLOAD_URL} && \
    tar zxf apache-tomcat-*.tar.gz && \
    rm apache-tomcat-*.tar.gz && \
    mv apache-tomcat* ${CATALINA_HOME}

# Remove unneeded apps and files
RUN rm -rf ${CATALINA_HOME}/webapps/examples ${CATALINA_HOME}/webapps/docs ${CATALINA_HOME}/webapps/ROOT ${CATALINA_HOME}/webapps/host-manager ${CATALINA_HOME}/RELEASE-NOTES ${CATALINA_HOME}/RUNNING.txt ${CATALINA_HOME}/bin/*.bat  ${CATALINA_HOME}/bin/*.tar.gz

RUN apt-get clean autoclean && \
    apt-get autoremove -y && \
    rm -rf /var/lib/{apt,dpkg,cache,log}/
    
EXPOSE 8080
EXPOSE 8009

VOLUME "/opt/tomcat/webapps"
WORKDIR /opt/tomcat

CMD ["/opt/tomcat/bin/tomcat.sh"]

# Launch Tomcat
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
