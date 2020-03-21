FROM centos:centos6

#get the latest version and update the packages
RUN yum -y update
RUN yum install -y epel-release && yum groupinstall -y 'development tools'

#install nginx
RUN yum -y install java nginx
RUN yum --enablerepo=epel install -y supervisor
RUN mv -f /etc/supervisord.conf /etc/supervisord.conf.org
ADD supervisord.conf /etc/

#install tomcat
RUN mkdir /opt/tomcat/
WORKDIR /opt/tomcat
RUN curl -O https://mirrors.estointernet.in/apache/tomcat/tomcat-8/v8.5.53/bin/apache-tomcat-8.5.53.tar.gz
RUN tar xvfz apache*.tar.gz
RUN mv apache-tomcat-8.5.53/* /opt/tomcat/.

#deploy sample war file
COPY sample.war /opt/tomcat/webapps/sample.war

#deploy nginx reverse proxy config file
COPY default.conf /etc/nginx/conf.d/default.conf

#copy supervisor configuration file to start multiple services 
# i.e. tomcat and nginx
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

EXPOSE 80
