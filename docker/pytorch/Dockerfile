FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /root

ENV TIME_ZONE Asia/Shanghai

# apt换源
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list && \
    sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && \
    sed -i 's/http:/https:/g' /etc/apt/sources.list && \
    apt update

# 设置时区
RUN apt install -y tzdata && \
    ln -snf /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && \
    echo ${TIME_ZONE} > /etc/timezone

# 下载vim, curl, git, python
ARG PYTHON='3.10.0'

RUN apt install -y build-essential gdb lcov pkg-config \
    libbz2-dev libffi-dev libgdbm-dev libgdbm-compat-dev liblzma-dev \
    libncurses5-dev libreadline6-dev libsqlite3-dev libssl-dev \
    lzma lzma-dev tk-dev uuid-dev zlib1g-dev \
    wget vim curl git
RUN wget https://registry.npmmirror.com/-/binary/python/$PYTHON/Python-$PYTHON.tgz
RUN tar -zxvf Python-$PYTHON.tgz && \
    cd Python-$PYTHON && \
    ./configure --enable-optimizations && \
    make && make install

RUN rm -rf Python-$PYTHON && \
    rm -f Python-$PYTHON.tgz

RUN ln -s /usr/local/bin/python3.10 /usr/bin/python && \
    ln -s /usr/local/bin/pip3.10 /usr/bin/pip

# 设置pypi清华源
RUN pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
RUN pip install --upgrade pip

# 下载pytorch
ARG TORCH='2.0.0'
ARG TORCHVISION='0.15.1'
ARG TORCHAUDIO='2.0.1'
#ARG CUDA='cu118'

RUN pip install torch==$TORCH torchvision==$TORCHVISION torchaudio==$TORCHAUDIO

# 下载 funasr 和 modelscope
RUN pip install funasr modelscope

# 复制代码
COPY . /root/

# 下载其他包
RUN pip install -r requirements.txt

