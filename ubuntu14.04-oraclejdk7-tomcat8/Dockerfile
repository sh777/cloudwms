FROM ubuntu:14.04
MAINTAINER YiFeng Huang "withoutbug@gmail.com"

ENV TOMCAT_DOWNLOAD_URL http://apache.mirrors.ionfish.org/tomcat/tomcat-8/v8.0.41/bin/apache-tomcat-8.0.41.tar.gz
ENV CATALINA_HOME /opt/tomcat

ENV ZOOKEEPER_DOWNLOAD_URL http://apache.osuosl.org/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
ENV ZOOKEEPER_HOME /opt/zookeeper

ENV CLOUDWMS_REPO_URL https://anobug:h9wAJWUCrZzHpvmm7drR@bitbucket.org/AI-CORE/cloudwms-src.git
ENV CLOUDWMS_HOME /opt/cloudwms

RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections

# Install Oracle JDK 7, Maven, Git
RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common && \
    add-apt-repository ppa:webupd8team/java && \
    apt-get update && \
    apt-get install -y --no-install-recommends oracle-java7-installer && \
    apt-get -y install maven && \
    apt-get -y install git && \
    rm -rf /var/lib/apt/lists/*
    
RUN apt-get clean autoclean && \
    apt-get autoremove -y && \
    rm -rf /var/lib/{apt,dpkg,cache,log}/
    
# Install Tomcat 8
RUN wget -q ${TOMCAT_DOWNLOAD_URL} && \
    tar zxf apache-tomcat-*.tar.gz && \
    rm apache-tomcat-*.tar.gz && \
    mv apache-tomcat* ${CATALINA_HOME}
    
# Remove unneeded apps and files
RUN rm -rf ${CATALINA_HOME}/webapps/examples ${CATALINA_HOME}/webapps/docs ${CATALINA_HOME}/webapps/ROOT ${CATALINA_HOME}/webapps/host-manager ${CATALINA_HOME}/RELEASE-NOTES ${CATALINA_HOME}/RUNNING.txt ${CATALINA_HOME}/bin/*.bat  ${CATALINA_HOME}/bin/*.tar.gz

# Install ZooKeeper
RUN wget -q ${ZOOKEEPER_DOWNLOAD_URL} && \
    tar zxf zookeeper-*.tar.gz && \
    rm zookeeper-*.tar.gz && \
    mv zookeeper* ${ZOOKEEPER_HOME} && \
    cp ${ZOOKEEPER_HOME}/conf/zoo_sample.cfg ${ZOOKEEPER_HOME}/conf/zoo.cfg

# Git source code    
RUN git clone ${CLOUDWMS_REPO_URL} ${CLOUDWMS_HOME} && \
    cd ${CLOUDWMS_HOME} && \
    pwd & \
    build.sh

EXPOSE 8080
EXPOSE 8009

VOLUME "/opt/tomcat/webapps"
WORKDIR /opt/tomcat

# Modify these options as needed
# ENV JAVA_OPTS="-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom"
# ENV CATALINA_OPTS="-Xms512M -Xmx1024M -server -XX:+UseParallelGC"

# Launch Tomcat
CMD ["/opt/tomcat/bin/catalina.sh", "run"]
