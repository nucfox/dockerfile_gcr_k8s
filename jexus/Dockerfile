FROM debian:stretch-slim

USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
                wget \
		bzip2 \
		unzip \
		xz-utils \
                xfonts-utils \
                fontconfig \
	&& rm -rf /var/lib/apt/lists/*

ENV LANG C.UTF-8

ADD winfonts /usr/share/fonts/winfonts
RUN cd /usr/share/fonts/winfonts \
       && /usr/bin/mkfontscale \
       && /usr/bin/mkfontdir \
       && /usr/bin/fc-cache
RUN cd /tmp
RUN wget --no-check-certificate https://linuxdot.net/down/jexus-5.8.3-x64.tar.gz
RUN tar -zxvf jexus-5.8.3-x64.tar.gz
RUN cp -rf jexus /usr/
RUN cd /usr/jexus
RUN bash /usr/jexus/jws regsvr
RUN rm -rf /tmp/jexus
RUN rm -rf /tmp/jexus-5.8.* >/dev/null 2>&1


EXPOSE 80
WORKDIR /usr/jexus
ENTRYPOINT ["/usr/jexus/jwss"]
