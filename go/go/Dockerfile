FROM centos:7

MAINTAINER kim

# 安装编译库
RUN yum install -y gcc gcc-c++ gd-devel libxml2-devel libcurl-devel libjpeg-devel libpng-devel openssl-devel make perl autoconf wget unzip

RUN yum -y install git


COPY CMake.zip /tmp
RUN cd /tmp && unzip CMake.zip && cd CMake && ./bootstrap && gmake && gmake install

### qconf and qconf_manager
### 启动容器后：
### 1、vi /usr/local/qconf/conf/idc.conf (如果zk不是安装在当前机器上，需要修改zk地址)
### 2、需要手动启动qconf：sh /usr/local/qconf/bin/agent-cmd.sh start
ADD QConf-1.2.1.tar.gz /tmp
RUN cd /tmp/QConf-1.2.1 && mkdir build && cd build && cmake .. && make && make install \
    && cp /usr/local/qconf/lib/libqconf.so /lib64/libqconf.so


RUN cd /tmp && wget https://golang.google.cn/dl/go1.14.2.linux-amd64.tar.gz \
	&& tar -C /usr/local -zxvf go1.14.2.linux-amd64.tar.gz

ENV GOROOT /usr/local/go
ENV PATH $PATH:$GOROOT/bin
ENV GOPATH /go
ENV PATH $PATH:$GOPATH/BIN

RUN go env -w GO111MODULE=on \
    && go env -w GOPROXY=https://goproxy.cn,direct

EXPOSE 8080
