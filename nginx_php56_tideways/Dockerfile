#Centos7+Nginx+PHP7.1
FROM centos:centos7

MAINTAINER Burgess Chen <loveshe76119@gmail.com>

#environment
ENV SSH_PASSWORD=test123
ENV TIDEWAYS_API_KEY={YOUR TIDEWAYS API KEY}
ENV TIDEWAYS_SAMPLE_RATE=25
ENV TIDEWAYS_FRAMEWORK=yii
ENV TIDEWAYS_SERVICE=web_docker

RUN yum -y update

#install base tools
RUN yum -y install dos2unix net-tools vixie-cron crontabs telnet screen wget unzip tar vim
RUN yum -y install epel-release git gcc

#install ssh service
RUN yum -y install openssh-server passwd initscripts
RUN echo "${SSH_PASSWORD}" | passwd "root" --stdin
RUN /usr/sbin/sshd-keygen -A

#Add the ngix & php repository
ADD nginx.repo /etc/yum.repos.d/nginx.repo
RUN dos2unix /etc/yum.repos.d/nginx.repo

#installing nginx 
RUN yum -y install nginx
ADD default.conf /etc/nginx/conf.d/default.conf
ADD index.html /var/www/html/

#install php
RUN wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
RUN wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN rpm -Uvh epel-release-latest-7.noarch.rpm
RUN rpm -Uvh remi-release-7.rpm

RUN yum -y install yum-utils openssl-devel
RUN yum-config-manager --enable remi-php56
RUN yum -y install php php-devel php-fpm php-gd php-mbstring php-pdo php-pecl-swoole php-pgsql php-mcrypt php-pear

RUN sed -ie 's/;date.timezone =/date.timezone = Asia\/Taipei/g' /etc/php.ini

#set php-fpm
COPY www.conf /etc/php-fpm.d/www.conf

#install composer
RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer
	
#install tideways
ADD tideways.repo /etc/yum.repos.d/tideways.repo
RUN rpm --import https://s3-eu-west-1.amazonaws.com/qafoo-profiler/packages/EEB5E8F4.gpg
RUN yum makecache --disablerepo=* --enablerepo=tideways
RUN yum -y install tideways-php tideways-cli tideways-daemon

RUN echo "tideways.api_key=${TIDEWAYS_API_KEY}" >> /etc/php.ini
RUN echo "tideways.sample_rate=${TIDEWAYS_SAMPLE_RATE}" >> /etc/php.ini
RUN echo "tideways.framework=${TIDEWAYS_FRAMEWORK}" >> /etc/php.ini
RUN echo "tideways.service=${TIDEWAYS_SERVICE}" >> /etc/php.ini

#installing supervisor
RUN mkdir /var/log/supervisor
RUN yum install -y python-setuptools
RUN easy_install pip
RUN pip install supervisor	


# Adding the config of the Supervisor
ADD supervisord.conf /etc/

#Start run supervisor 
CMD ["supervisord", "-n"]

















