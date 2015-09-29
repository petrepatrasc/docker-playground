FROM ubuntu:15.04

# Noninteractive installs
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections
RUN locale-gen en_US.UTF-8
ENV LANG       en_US.UTF-8
ENV LC_ALL     en_US.UTF-8
ENV HOME /root
ENV DEBIAN_FRONTEND noninteractive

# Require packages
RUN \
	apt-get install -y \
		git \
		git-core \
		vim \
		curl \
		software-properties-common

# Enable some repos and upgrade OS
RUN \
	apt-add-repository multiverse && \
	apt-get upgrade -y && \
	apt-get update

# Install Apache
ENV APACHE_RUN_USER www-data 
ENV APACHE_RUN_GROUP www-data 
ENV APACHE_LOG_DIR /var/log/apache2
ENV APP_DOCUMENT_ROOT /var/www

RUN \
	apt-get install -y \
		apache2-mpm-event \
		libapache2-mod-fastcgi

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf
RUN a2enmod rewrite fastcgi alias && service apache2 restart
ADD start-apache2.sh /root/start-apache2.sh

# Install PHP
RUN \
	add-apt-repository ppa:ondrej/php5-5.6 && \
	apt-get update && \
	apt-get install -y \
		php5-fpm \
		php5-curl

# Supervisor
RUN apt-get install -y supervisor
ADD supervisord-apache2.conf /etc/supervisor/conf.d/supervisord-apache2.conf

# Start up
EXPOSE 80
ADD system-init.sh /root/system-init.sh
CMD ["/root/system-init.sh"]
