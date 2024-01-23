FROM nvidia/cuda:12.1.1-cudnn8-runtime-ubuntu22.04

# 设置环境变量
ENV PATH="/opt/miniconda/bin:$PATH"
ENV PYTHON_VERSION=3.11.7

WORKDIR /temp

# 安装基础包
RUN \
    apt update && \
    apt install -y \
        sudo curl wget git vim unzip zip \
        build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev \
        libreadline-dev libffi-dev libsqlite3-dev libbz2-dev liblzma-dev && \
    apt clean && \
    rm -rf /var/lib/apt/lists/*

# install python
# https://www.python.org/ftp/python/

RUN wget https://www.python.org/ftp/python/"$PYTHON_VERSION"/Python-"$PYTHON_VERSION".tgz && \
    tar -xvf Python-"$PYTHON_VERSION".tgz && \
    cd Python-"$PYTHON_VERSION" && \
    ./configure --enable-optimizations && \
    make && \
    make install && \
    ln -s /usr/local/bin/python3 /usr/local/bin/python && \
    ln -s /usr/local/bin/pip3 /usr/local/bin/pip

# 安装Miniconda
# 创建llm-dev环境并安装PyTorch
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/miniconda && \
    conda create -n llm-dev python="$PYTHON_VERSION" && \
    conda init bash && \
    conda activate llm-dev && \
    conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia


WORKDIR /workspace

RUN rm -r /temp

