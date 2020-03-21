FROM centos:centos6

# Get images and update packages
RUN yum -y update
RUN yum install -y epel-release && yum groupinstall -y 'development tools'
RUN yum install -y zlib-dev openssl-devel sqlite-devel bzip2-devel xz-libs wget

# Install python 2.7
RUN wget https://www.python.org/ftp/python/2.7.8/Python-2.7.8.tar.xz
RUN tar xvfJ Python-2.7.8.tar.xz
WORKDIR Python-2.7.8
RUN ./configure --prefix=/usr/local && make && make altinstall
RUN curl https://bootstrap.pypa.io/get-pip.py | python2.7
RUN alias python=/usr/local/bin/python2.7

# Add repository for mongodb
ADD mongodb-org-4.0.repo /etc/yum.repos.d

# Install mongodb
RUN yum -y install mongodb-org

# Create database directory for mongodb
RUN mkdir -p /data/db 

# Install java for tomcat
RUN yum -y install java

# Install supervisor to run multiple services inside a container
RUN yum --enablerepo=epel install -y supervisor
RUN mv -f /etc/supervisord.conf /etc/supervisord.conf.org
ADD supervisord.conf /etc/

# Install Tomcat
RUN mkdir /opt/tomcat/

WORKDIR /opt/tomcat
RUN curl -O http://apachemirror.wuchna.com/tomcat/tomcat-7/v7.0.103/bin/apache-tomcat-7.0.103.tar.gz
RUN tar xvfz apache*.tar.gz
RUN mv apache-tomcat-7.0.103/* /opt/tomcat/.

# Copy mongodb configuration file
COPY mongod.conf /etc/mongod.conf

# Copy supervisor configuration file 
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

EXPOSE 27017 8080
