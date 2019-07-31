FROM centos:7.5.1804

# set label
LABEL maintainer="willlu <willluzheng@163.com>"

# set environment
ENV SEATA_USER="seata" \
    BASE_DIR="/home/seata" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    JAVA="/usr/lib/jvm/java-1.8.0-openjdk/bin/java" \
    TIME_ZONE="Asia/Shanghai"

ARG SEATA_VERSION=0.7.1

WORKDIR /$BASE_DIR

COPY jar/seata-server-${SEATA_VERSION}.tar.gz /home

RUN set -x \
    && yum update -y \
    && yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel wget iputils nc vim libcurl \
    && mkdir /opt/seata \
    && tar -xzvf /home/seata-server-${SEATA_VERSION}.tar.gz -C /opt/seata \
    && rm -rf /home/seata-server-${SEATA_VERSION}.tar.gz /opt/seata/bin/* /opt/seata/conf/*.conf  \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo '$TIME_ZONE' > /etc/timezone \
    && yum clean all

# 设置额外参数
ENV EXTRA_JVM_ARGUMENTS="-XX:MaxDirectMemorySize=1024M"

ADD bin/seata-server.sh /opt/seata/bin/seata-server.sh
ADD conf/file.conf /opt/seata/conf/file.conf
ADD conf/registry.conf /opt/seata/conf/registry.conf

ENTRYPOINT ["sh","/opt/seata/bin/seata-server.sh"]
EXPOSE 8091

