#MariaDB
FROM centos:centos7

MAINTAINER Burgess Chen <loveshe76119@gmail.com>

#environment
ENV SSH_PASSWORD=test123

RUN yum -y update

#install base tools
RUN yum -y install dos2unix net-tools vixie-cron crontabs telnet screen wget unzip tar vim
RUN yum -y install epel-release git gcc

#install ssh service
RUN yum -y install openssh-server passwd initscripts
RUN echo "${SSH_PASSWORD}" | passwd "root" --stdin
RUN /usr/sbin/sshd-keygen -A

#Add the mariadb repository
ADD mariadb.repo /etc/yum.repos.d/mariadb.repo
RUN dos2unix /etc/yum.repos.d/mariadb.repo

#install mariaDB
RUN yum -y install mariadb-server mariadb-client
RUN mkdir /var/log/mysql
RUN chown mysql:mysql /var/log/mysql

COPY my.cnf /etc/my.cnf
ADD initial.sql /home/

RUN /etc/init.d/mysql start &&\
	mysql < /home/initial.sql

#RUN /etc/init.d/mysql start
#RUN mysql < /home/initial.sql
#RUN /etc/init.d/mysql stop

#installing supervisor
RUN mkdir /var/log/supervisor 
RUN yum install -y python-setuptools
RUN easy_install pip
RUN pip install supervisor	

# Adding the config of the Supervisor
ADD supervisord.conf /etc/

#Start run supervisor 
CMD ["supervisord", "-n"]









