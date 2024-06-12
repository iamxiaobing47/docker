FROM ubuntu:latest
LABEL authors="xiaobing"

ENV JAVA_HOME=/usr/local/jdk8
ENV PATH=$PATH:$JAVA_HOME/bin

RUN apt-get update && \
    apt-get install net-tools && \
    apt install -y wget

RUN cd /usr/local && \
    wget https://builds.openlogic.com/downloadJDK/openlogic-openjdk/8u412-b08/openlogic-openjdk-8u412-b08-linux-x64.tar.gz && \
    tar -xzf openlogic-openjdk-8u412-b08-linux-x64.tar.gz && \
    mv openlogic-openjdk-8u412-b08-linux-x64 jdk8 && \
    rm -f openlogic-openjdk-8u412-b08-linux-x64.tar.gz && \
    rm -rf openlogic-openjdk-8u412-b08-linux-x64


## configure JDK8