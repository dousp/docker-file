FROM nvidia/cuda:12.1.1-cudnn8-runtime-ubuntu22.04

# 指定使用bash
#RUN rm /bin/sh && ln -s /bin/bash /bin/sh
SHELL ["/bin/bash", "-c"]

# 设置环境变量

ENV PYTHON_VERSION=3.11.7

WORKDIR /temp

# 安装基础包
RUN \
    rm -f /etc/apt/sources.list.d/cuda*.list && \
    apt-key del 7fa2af80 && \
    apt-get update && \
    apt-get clean

RUN \
    apt-get install -y --no-install-recommends sudo wget && \
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb && \
    sudo dpkg -i cuda-keyring_1.0-1_all.deb && \
    apt-get install -y --no-install-recommends  \
        curl git vim unzip zip \
        build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev \
        libreadline-dev libffi-dev libsqlite3-dev libbz2-dev liblzma-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# install python
# https://www.python.org/ftp/python/
#RUN wget https://www.python.org/ftp/python/"$PYTHON_VERSION"/Python-"$PYTHON_VERSION".tgz && \
#    tar -xvf Python-"$PYTHON_VERSION".tgz && \
#    cd Python-"$PYTHON_VERSION" && \
#    ./configure --enable-optimizations && \
#    make && \
#    make install && \
#    ln -s /usr/local/bin/python3 /usr/local/bin/python && \
#    ln -s /usr/local/bin/pip3 /usr/local/bin/pip

# 安装Miniconda
# 创建llm-dev环境并安装PyTorch
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/miniconda

#ENV PATH="/opt/miniconda/bin:$PATH"

RUN \
    conda update -y conda && \
    conda create -n llm-dev python="$PYTHON_VERSION"

#ENV PATH /opt/miniconda/envs/llm-dev/bin:$PATH
#ENV CONDA_DEFAULT_ENV llm-dev

SHELL ["conda", "run", "--no-capture-output", "-n", "llm-dev", "/bin/bash", "-c"]

RUN conda activate llm-dev
#    conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia -y && \
#    conda clean -afy


WORKDIR /workspace

RUN rm -r /temp

