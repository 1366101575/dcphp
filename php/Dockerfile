FROM centos:7

MAINTAINER kim

# 安装编译库
RUN yum install -y gcc gcc-c++ gd-devel libxml2-devel libcurl-devel libjpeg-devel libpng-devel openssl-devel make perl autoconf wget unzip
RUN yum install epel-release && yum update && yum install libmcrypt libmcrypt-devel mcrypt mhash

# 下载压缩包解压到指定目录
ADD php-5.6.40.tar.gz /tmp/

# 编译安装php，执行相关命令，PHP7安装qconf_manager扩展时有坑（暂时没踩过去）
RUN cd /tmp/php-5.6.40 && \
    ./configure --prefix=/usr/local/php \
    --with-config-file-path=/usr/local/php/etc \
    --with-mysql --with-mysqli --with-pdo-mysql \
    --with-openssl --with-zlib --with-curl --with-gd \
    --with-jpeg-dir --with-png-dir --with-iconv --with-mcrypt \
    --enable-fpm --enable-zip --enable-mbstring --enable-bcmath && \
    make -j 4 && make install && \
    cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf && \
    sed -i "s/127.0.0.1/0.0.0.0/" /usr/local/php/etc/php-fpm.conf && \
    cp ./sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && \
    chmod +x /etc/init.d/php-fpm

# 复制本地配置文件到容器内
COPY php.ini /usr/local/php/etc

COPY CMake.zip /tmp
RUN cd /tmp && unzip CMake.zip && cd CMake && ./bootstrap && gmake && gmake install

### qconf and qconf_manager
ADD QConf-1.2.1.tar.gz /tmp
RUN cd /tmp/QConf-1.2.1 && mkdir build && cd build && cmake .. && make && make install && \
    cd ../driver/php && /usr/local/php/bin/phpize && \
    ./configure --with-php-config=/usr/local/php/bin/php-config \
    --with-libqconf-dir=/usr/local/qconf/include/ \
    --enable-static LDFLAGS=/usr/local/lib/libqconf.a && \
    make && make install && \
    echo "extension=qconf.so" >> /usr/local/php/etc/php.ini && \
    cd /tmp/QConf-1.2.1/manager/ && mkdir build && cd build && cmake .. && make && make install && \
    cd ../src/php/ && /usr/local/php/bin/phpize && \
    ./configure --with-php-config=/usr/local/php/bin/php-config --with-qconfzk-dir=/usr/local/qconf/manage/include/ --enable-static LDFLAGS=/usr/local/qconf/manage/lib/libqconf_zk.a && \
    make && make install && \
    echo "extension=qconf_manager.so" >> /usr/local/php/etc/php.ini

### redis
RUN cd /tmp && wget https://pecl.php.net/get/redis-4.0.0.tgz && \
    tar -zxvf redis-4.0.0.tgz && cd redis-4.0.0 && /usr/local/php/bin/phpize && \
    ./configure --with-php-config=/usr/local/php/bin/php-config && \
    make && make install && \
    echo "extension=redis.so" >> /usr/local/php/etc/php.ini

EXPOSE 9000
CMD /etc/init.d/php-fpm start  && tail -F /var/log/messages