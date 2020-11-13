FROM ubuntu:20.04

LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com> Modified By jiang9217@foxmail.com"

# ENV http_proxy=http://proxy.it.taikang.com:8080

RUN apt-get update
RUN apt-get install -y gcc \
			libc-devel \
			make \
			openssl-devel \
			pcre-devel \
			zlib-devel \
			libxslt-devel \
			perl-devel \
			libedit-devel \
			openjdk-11-jre-headless
RUN mkdir -p /nginx/src \
	&& curl -SL https://nginx.org/download/nginx-1.12.2.tar.gz \
	tar zxf nginx-1.12.2.tar.gz /nginx/src
WORKDIR /nginx/src/nginx-1.12.2
RUN ./configure --with-pcre --with-http_ssl_module --with-stream \
	&& make \
	&& make install


