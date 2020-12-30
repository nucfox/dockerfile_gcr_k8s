FROM ubuntu:20.04

LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com> Modified By jiang9217@foxmail.com"

RUN set -x \
	&& apt-get update \
	&& apt-get install -y gcc \
	                git \
			make \
			curl \
			libpcre3 \
			libpcre3-dev \
			zlib1g-dev \
			openssl \
			libssl-dev \
			iproute2 \
			openjdk-11-jre-headless \
			
	&& rm -rf /var/lib/apt/lists/* \
	&& mkdir -p /tmp/openresty_src \
	&& cd /tmp/openresty_src \
	&& curl -O https://openresty.org/download/openresty-1.19.3.1.tar.gz \
	&& curl -o nginx-module-vts-0.1.18.tar.gz https://codeload.github.com/vozlt/nginx-module-vts/tar.gz/v0.1.18 \
	&& tar zxf openresty-1.19.3.1.tar.gz \
	&& tar zxf nginx-module-vts-0.1.18.tar.gz \
	&& rm -f openresty-1.19.3.1.tar.gz \
	&& rm -f nginx-module-vts-0.1.18.tar.gz \
	&& cd openresty-1.19.3.1 \
	&& ./configure --add-module=../nginx-module-vts-0.1.18 \
        && make \
        && make install \
	&& cd /tmp \
	&& rm -rf openresty_src \
	&& ln -s /usr/local/openresty/nginx/sbin/nginx /usr/bin/ \
	&& cd /usr/local/openresty \
	&& git clone https://github.com/knyar/nginx-lua-prometheus.git

ENV PATH=$PATH:/usr/local/openresty/luajit/bin:/usr/local/openresty/nginx/sbin:/usr/local/openresty/bin
WORKDIR /usr/local/openresty
COPY luacustom ./luacustom

