FROM centos:7.5.1804

# set label
LABEL maintainer="willlu <willluzheng@163.com>"

# set environment
ENV BASE_DIR="/home/seata" \
    JAVA_HOME="/usr/lib/jvm/java-1.8.0-openjdk" \
    JAVA="/usr/lib/jvm/java-1.8.0-openjdk/bin/java" \
    TIME_ZONE="Asia/Shanghai" \
    SEATA_MODE=db

ARG SEATA_VERSION=0.7.1

WORKDIR /$BASE_DIR

COPY jar/seata-server-${SEATA_VERSION}.tar.gz /home

RUN set -x \
    && yum update -y \
    && yum install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel wget iputils nc vim libcurl \
    && mkdir /opt/seata \
    && tar -xzvf /home/seata-server-${SEATA_VERSION}.tar.gz -C $BASE_DIR \
    && rm -rf /home/seata-server-${SEATA_VERSION}.tar.gz $BASE_DIR/bin/* $BASE_DIR/conf/*.conf  \
    && ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo '$TIME_ZONE' > /etc/timezone \
    && yum clean all

# 设置额外参数
ENV EXTRA_JVM_ARGUMENTS="-XX:MaxDirectMemorySize=1024M"

ADD bin/seata-server.sh $BASE_DIR/bin/seata-server.sh
ADD conf/file.conf $BASE_DIR/conf/file.conf
ADD conf/registry.conf $BASE_DIR/conf/registry.conf

RUN chmod a+x $BASE_DIR/bin/seata-server.sh

#ENTRYPOINT ["sh","/opt/seata/bin/seata-server.sh", "-m", "$SEATA_MODE"]
ENTRYPOINT [ "sh", "-c", "$BASE_DIR/bin/seata-server.sh -m $SEATA_MODE" ]

EXPOSE 8091 9898