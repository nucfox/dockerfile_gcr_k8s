FROM jenkins/jenkins:lts

USER root
#切换成阿里云的debian 9.x (stretch)源
#RUN cat > /etc/apt/sources.list << EOF
#deb http://mirrors.aliyun.com/debian/ stretch main non-free contrib
#deb-src http://mirrors.aliyun.com/debian/ stretch main non-free contrib
#deb http://mirrors.aliyun.com/debian-security stretch/updates main
#deb-src http://mirrors.aliyun.com/debian-security stretch/updates main
#deb http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
#deb-src http://mirrors.aliyun.com/debian/ stretch-updates main non-free contrib
#deb http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib
#deb-src http://mirrors.aliyun.com/debian/ stretch-backports main non-free contrib
#EOF
#更新源并安装缺少的包
RUN apt-get update && apt-get install -y libltdl7

ARG dockerGid=993

RUN echo "dockerroot:x:${dockerGid}:jenkins" >> /etc/group 

# 安装development tools
RUN apt-get install -y gcc g++ make
# 安装npm
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash -
RUN apt-get install -y nodejs
RUN npm install -g cnpm --registry=https://registry.npm.taobao.org

# 安装ansble
RUN echo "deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main" >> /etc/apt/sources.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
RUN apt-get update
RUN apt-get install -y ansible
RUN echo "host_key_checking = False" >> /etc/ansible/ansible.cfg

# 安装 docker-compose 构建环境需要
RUN curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 安装 mono
RUN apt-get install -y apt-transport-https dirmngr
RUN apt-key adv --no-tty --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN echo "deb https://download.mono-project.com/repo/debian stable-stretch main" > /etc/apt/sources.list.d/mono-official-stable.list
RUN apt-get update
RUN apt-get install -y mono-devel mono-complete mono-dbg referenceassemblies-pcl ca-certificates-mono mono-xsp4
# 安装文件名编码转换工具
RUN apt-get install -y convmv
# 安装NuGet
RUN curl -o /usr/bin/nuget.exe https://dist.nuget.org/win-x86-commandline/latest/nuget.exe
RUN echo "#!/bin/sh\nMONO_GC_PARAMS=\"nursery-size=64m,\$MONO_GC_PARAMS\" exec /usr/bin/mono \$MONO_OPTIONS /usr/bin/nuget.exe \"\$@\"" > /usr/bin/nuget 
RUN chmod +x /usr/bin/nuget

# Install .NET CLI dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        libc6 \
        libcurl3 \
        libgcc1 \
        libgssapi-krb5-2 \
        libicu57 \
        liblttng-ust0 \
        libssl1.0.2 \
        libstdc++6 \
        libunwind8 \
        libuuid1 \
        zlib1g \
    && rm -rf /var/lib/apt/lists/*

# Install .NET Core SDK
ENV DOTNET_SDK_VERSION 2.2.105
ENV DOTNET_SDK_DOWNLOAD_URL https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-linux-x64.tar.gz
ENV DOTNET_SDK_DOWNLOAD_SHA b7ad26b344995de91848adec56bda5dfe5fef0b83abaa3e4376dc790cf9786e945b625de1ae4cecaf5c5bef86284652886ed87696581553aeda89ee2e2e99517

RUN curl -SL $DOTNET_SDK_DOWNLOAD_URL --output dotnet.tar.gz \
    && echo "$DOTNET_SDK_DOWNLOAD_SHA dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf dotnet.tar.gz -C /usr/share/dotnet \
    && rm dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

# Enable detection of running in a container
ENV DOTNET_RUNNING_IN_CONTAINER=true \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip

# Trigger the population of the local package cache
RUN mkdir warmup \
    && cd warmup \
    && dotnet new \
    && cd .. \
    && rm -rf warmup \
    && rm -rf /tmp/NuGetScratch

# Workaround for https://github.com/Microsoft/DockerTools/issues/87. This instructs NuGet to use 4.5 behavior in which
# all errors when attempting to restore a project are ignored and treated as warnings instead. This allows the VS
# tooling to use -nowarn:MSB3202 to ignore issues with the .dcproj project
ENV RestoreUseSkipNonexistentTargets false

USER jenkins
