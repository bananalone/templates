FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /root

# 设置字符集
ENV LANG=C.UTF-8 \
    LANGUAGE=C.UTF-8 \
    LC_ALL=C.UTF-8

# apt 换源
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
    sed -i 's/http:/https:/g' /etc/apt/sources.list && \
    apt update

# 设置时区
ENV TIME_ZONE=Asia/Shanghai
RUN apt install -y tzdata && \
    ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && \
    echo $TIME_ZONE > /etc/timezone

# 下载 vim, curl, git, python
ARG PYTHON='3.11.8'
RUN apt install -y build-essential gdb lcov pkg-config \
    libbz2-dev libffi-dev libgdbm-dev libgdbm-compat-dev liblzma-dev \
    libncurses5-dev libreadline6-dev libsqlite3-dev libssl-dev \
    lzma lzma-dev tk-dev uuid-dev zlib1g-dev \
    wget vim curl git
RUN wget https://registry.npmmirror.com/-/binary/python/$PYTHON/Python-$PYTHON.tgz
RUN tar -zxvf Python-$PYTHON.tgz && \
    cd Python-$PYTHON && \
    ./configure --enable-optimizations && \
    make && make install && \
    ln -s /usr/local/bin/python3 /usr/bin/python && \
    ln -s /usr/local/bin/pip3 /usr/bin/pip && \
    rm -rf /root/Python-$PYTHON && \
    rm -f /root/Python-$PYTHON.tgz

# 下载 poetry
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple && \
    pip install -U pip setuptools && \
    pip install poetry && \
    poetry completions bash >> ~/.bash_completion

