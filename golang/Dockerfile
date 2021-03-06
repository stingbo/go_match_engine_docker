FROM golang
LABEL maintainer="sting_bo@163.com"
ENV DEBIAN_FRONTEND noninteractive

# 修改镜像源
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak \
    && sed -i "s@http://deb.debian.org@http://mirrors.aliyun.com@g" /etc/apt/sources.list \
    && sed -i "s@security.debian.org/debian-security@mirrors.aliyun.com/debian-security@g" /etc/apt/sources.list \
    && cat /etc/apt/sources.list \
    && apt-get update --fix-missing

ENV PROTOBUF_VERSION 3.13.0
ENV PROTOBUF protobuf-all-${PROTOBUF_VERSION}.tar.gz
ENV PROTOBUF_PATH protobuf-${PROTOBUF_VERSION}
COPY $PROTOBUF ./

RUN buildDeps=" \
    " \
    runtimeDeps=" \
        autoconf \
        automake \
        libtool \
        curl \
        make \
        g++ \
        unzip \
        vim \
        curl \
        git \
        supervisor \
    " \
    && apt-get install --assume-yes apt-utils -y $buildDeps $runtimeDeps --no-install-recommends \
    && tar xfz $PROTOBUF \
    && cd $PROTOBUF_PATH && ./configure && make && make install && ldconfig /usr/local/lib \
    && cd ../ && rm -rf $PROTOBUF $PROTOBUF_PATH \
    && /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' >/etc/timezone \
    && rm -r /var/lib/apt/lists/*

WORKDIR $GOPATH/src
ENV GO111MODULE="on"
ENV GOPROXY="https://goproxy.cn"
#COPY ./supervisord.conf /etc/supervisor
#COPY ./start.sh /root/startup.sh
#ENV APP_FILE /root/startup.sh
#CMD $APP_FILE

EXPOSE 8088
