FROM nginx:stable

ENV TZ=Asia/Chongqing

#官方镜像太慢，换成网易源
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak && \
    ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    mkdir -pv /etc/nginx/vhost/upstream && \
    echo "deb http://mirrors.163.com/debian/ stretch main non-free contrib" > /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian/ stretch-backports main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >> /etc/apt/sources.list && \
    echo "deb-src http://mirrors.163.com/debian-security/ stretch/updates main non-free contrib" >> /etc/apt/sources.list

#RUN apt-get update && apt-get upgrade && apt-get install -y \
#    iputils-ping
#    ntpdate \
#    libconfig-crontab-perl

COPY ./config/nginx.1.conf /etc/nginx/nginx.conf

COPY ./config/server.* /etc/nginx/
COPY ./config/kf5.com.* /etc/nginx/

#CMD ['nginx-debug', '-g', 'daemon off;']
