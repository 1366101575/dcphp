FROM centos:7

MAINTAINER kim

# 安装编译库
RUN yum install -y gcc gcc-c++ gd-devel libxml2-devel libcurl-devel libjpeg-devel libpng-devel openssl-devel make perl autoconf wget unzip


COPY CMake.zip /tmp
RUN cd /tmp && unzip CMake.zip && cd CMake && ./bootstrap && gmake && gmake install

### qconf and qconf_manager
ADD QConf-1.2.1.tar.gz /tmp
RUN cd /tmp/QConf-1.2.1 && mkdir build && cd build && cmake .. && make && make install \
    && cp /usr/local/qconf/lib/libqconf.so /lib64/libqconf.so

RUN cd /tmp && wget https://golang.google.cn/dl/go1.14.2.linux-amd64.tar.gz && tar -zxvf go1.14.2.linux-amd64.tar.gz \
    && export GOROOT=/usr/local/go \
    && export PATH=$PATH:$GOROOT/bin \
    && export GOPATH=/go \
    && export PATH=$PATH:$GOPATH/BIN \
    && source /etc/profile \
    && go env -w GO111MODULE=on \
    && go env -w GOPROXY=https://goproxy.cn,direct \
    && go env -w GOPRIVATE=gl.zego.im \

EXPOSE 8090
