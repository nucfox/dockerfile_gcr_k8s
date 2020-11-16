FROM ubuntu:20.04

LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com> Modified By jiang9217@foxmail.com"

RUN set -x \
	&& apt-get update \
	&& apt-get install -y gcc \
			make \
			curl \
			libpcre3 \
			libpcre3-dev \
			zlib1g-dev \
			openssl \
			libssl-dev \
			openjdk-11-jre-headless \
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir -p /tmp/nginx_src \
	&& cd /tmp/nginx_src \
	&& curl -O https://nginx.org/download/nginx-1.16.1.tar.gz \
	&& tar -zxf nginx-1.16.1.tar.gz \
	&& rm -f nginx-1.16.1.tar.gz \
	&& cd /tmp/nginx_src/nginx-1.16.1 \
	&& ./configure --with-pcre --with-http_ssl_module --with-stream \
        && make \
        && make install \
	&& cd /tmp \
	&& rm -rf nginx_src \
	&& ln -s /usr/local/nginx/sbin/nginx /bin/
WORKDIR /usr/local/nginx
#RUN sed -i '/cd.current_salt\[0\] = \~salt\[0\];/{s/^\(.*\)$/\/\*\1\*\//}' src/os/unix/ngx_user.c

