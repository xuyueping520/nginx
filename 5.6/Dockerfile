FROM index.alauda.cn/library/centos:6.6

RUN rpm -Uvh http://dl.fedoraproject.org/pub/epel/epel-release-latest-6.noarch.rpm
RUN rpm -Uvh http://mirror.webtatic.com/yum/el6/latest.rpm

RUN yum makecache
RUN yum install -y \
	php56w \
	php56w-fpm \
	nginx \
	php56w-devel \
	gcc \
	gcc-c++ \
	make \
	openssl-devel \
	python-pip \
	git

RUN pip install supervisor supervisor-stdout && pip install --upgrade setuptools

# add php yaf ext

RUN git clone -b php5 https://github.com/laruence/php-yaf /tmp/php-yaf && \
	cd /tmp/php-yaf && \
	phpize && \
	./configure --with-php-config=/usr/bin/php-config && \
	make && make install && \
	echo -e '\nextension=yaf.so' >> /etc/php.ini && \
	cd / && rm -rf /tmp/php-yaf 

RUN sed -i "s/;date.timezone =.*/date.timezone = UTC/" /etc/php.ini
COPY nginx.conf /etc/nginx/nginx.conf
COPY supervisord.conf /etc/supervisord.conf
EXPOSE 80
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]
