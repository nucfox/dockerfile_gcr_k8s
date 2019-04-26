FROM microsoft/dotnet:2.2-sdk

USER root

RUN apt-get update && apt-get install -y --no-install-recommends \
                libgdiplus \
                xfonts-utils \
                fontconfig \
	&& rm -rf /var/lib/apt/lists/*

ENV LANG C.UTF-8

RUN ln -s /lib/x86_64-linux-gnu/libdl.so.2  /lib/x86_64-linux-gnu/libdl.so
RUN ln -s /usr/lib/libgdiplus.so /usr/lib/gdiplus.dll

ADD winfonts /usr/share/fonts/winfonts
RUN cd /usr/share/fonts/winfonts \
       && /usr/bin/mkfontscale \
       && /usr/bin/mkfontdir \
       && /usr/bin/fc-cache
