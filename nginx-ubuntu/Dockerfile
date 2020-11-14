FROM ubuntu:20.04

LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com> Modified By jiang9217@foxmail.com"

# ENV http_proxy=http://proxy.it.taikang.com:8080

RUN apt-get update
RUN apt-get install -y gcc \
			curl \
			libpcre3 \
			libpcre3-dev \
			zlib1g-dev \
			openssl \
			libssl-dev \
			openjdk-11-jre-headless
RUN mkdir -p /nginx/src \
	&& curl -O https://nginx.org/download/nginx-1.12.2.tar.gz \
	&& tar -zxf nginx-1.12.2.tar.gz
WORKDIR /nginx/src/nginx-1.12.2
RUN ./configure --with-pcre --with-http_ssl_module --with-stream \
	&& make \
	&& make install


