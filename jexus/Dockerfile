FROM debian:stretch-slim

USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
		bzip2 \
		unzip \
		xz-utils \
	&& rm -rf /var/lib/apt/lists/*

ENV LANG C.UTF-8

RUN curl https://jexus.org/release/x64/install.sh|sh

EXPOSE 80
WORKDIR /usr/jexus
ENTRYPOINT ["bash","/usr/jexus/jws","start"]
