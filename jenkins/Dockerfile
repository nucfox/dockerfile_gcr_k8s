FROM jenkins/jenkins

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

# 安装 docker-compose 因为等下构建环境的需要
RUN curl -L https://github.com/docker/compose/releases/download/1.22.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
RUN chmod +x /usr/local/bin/docker-compose

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# 安装 mono
RUN apt-get install -y apt-transport-https dirmngr
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
RUN echo "deb https://download.mono-project.com/repo/debian stable-stretch main" > /etc/apt/sources.list.d/mono-official-stable.list
RUN apt-get update
RUN apt-get install -y mono-devel mono-complete mono-dbg referenceassemblies-pcl ca-certificates-mono mono-xsp4
# 安装文件名编码转换工具
RUN apt-get install -y convmv

USER jenkins
