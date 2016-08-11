FROM ubuntu:14.04
MAINTAINER Prakash <prakashnd03.@gmail.com>

#Installing java not form docker image 
RUN     echo "THE JAVA_HOME IS : "+$JAVA_HOME && \
        apt-get update && \
        apt-get upgrade -y && \
        apt-get install -y  software-properties-common && \
        add-apt-repository ppa:webupd8team/java -y && \
        apt-get update && \
        echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
        apt-get install -y oracle-java7-installer && \
        echo "THE JAVA_HOME IS : "+$JAVA_HOME && \
        apt-get clean


ENV JAVA_HOME /usr/lib/jvm/java-7-oracle 
ENV JAVA_OPTS -XX:MaxPermSize=512m -Xms2048m -Xmx4096m



ENV MAVEN_VERSION 3.3.9

RUN apt-get update && apt-get install -y curl && apt-get clean

RUN mkdir -p /usr/share/maven \
  && curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
    | tar -xzC /usr/share/maven --strip-components=1 \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven 
ENV MAVEN_OPTS -XX:MaxPermSize=512m -Xms2048m -Xmx4096m
VOLUME /root/.m2


#CMD ["mvn"]


RUN mkdir -p /usr/src/app

#RUN export MAVEN_OPTS="-Xmx1024m -XX:MaxPermSize=1024m"  

VOLUME /usr/src/app
WORKDIR /usr/src/app

ADD . /usr/src/app
#RUN echo $MAVEN_OPTS
CMD ["mvn clean install"]


ENV TOMCAT_VERSION 7.0.70

RUN wget --quiet --no-cookies http://apache.rediris.es/tomcat/tomcat-7/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz -O /tmp/tomcat.tgz && \
tar xzvf /tmp/tomcat.tgz -C /opt && \
mv /opt/apache-tomcat-${TOMCAT_VERSION} /opt/tomcat && \
rm /tmp/tomcat.tgz && \
rm -rf /opt/tomcat/webapps/examples && \
rm -rf /opt/tomcat/webapps/docs && \
rm -rf /opt/tomcat/webapps/ROOT

#Add admin/admin user
COPY tomcat-users.xml /opt/tomcat/conf/
ENV CATALINA_HOME /opt/tomcat
ENV PATH $PATH:$CATALINA_HOME/bin

EXPOSE 8080
EXPOSE 8009
VOLUME "/opt/tomcat/webapps"
#WORKDIR /opt/tomcat

#COPY /usr/src/app/target/webAppTest.war /opt/tomcat/webapps
# Launch Tomcat
CMD ["/opt/tomcat/bin/catalina.sh", "run"]

