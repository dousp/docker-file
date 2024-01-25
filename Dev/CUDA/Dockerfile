FROM nvidia/cuda:12.1.1-cudnn8-runtime-ubuntu22.04

# 指定使用bash
#RUN rm /bin/sh && ln -s /bin/bash /bin/sh
SHELL ["/bin/bash", "-c"]

# 设置环境变量

ENV PYTHON_VERSION=3.11.7

WORKDIR /temp

# 安装基础包
RUN \
    echo  "alias mba='micromamba'" >> ~/.bash_profile && \
    echo  "alias pip='pip3'" >> ~/.bash_profile && \
    echo  "alias python='python3'" >> ~/.bash_profile && \
    rm -f /etc/apt/sources.list.d/cuda*.list && \
    apt-key del 7fa2af80 && \
    apt-get update && \
    apt-get install -y --no-install-recommends sudo wget && \
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb && \
    sudo dpkg -i cuda-keyring_1.0-1_all.deb && \
    apt-get install -y --no-install-recommends  \
        curl git vim unzip zip ffmpeg \
        build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev \
        libreadline-dev libffi-dev libsqlite3-dev libbz2-dev liblzma-dev && \
    apt-get install tzdata -y && ln -snf /usr/share/zoneinfo/"$TZ" /etc/localtime && echo "$TZ" > /etc/timezone &&\
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 安装Miniconda
# 创建llm-dev环境并安装PyTorch
ENV PATH="/opt/miniconda/bin:$PATH"
#ENV CONDA_DEFAULT_ENV llm-dev

RUN \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/miniconda  && \
    conda update -y conda && \
    conda create -n llm-dev python="$PYTHON_VERSION"

#ENV PATH /opt/miniconda/envs/llm-dev/bin:$PATH

SHELL ["conda", "run", "--no-capture-output", "-n", "llm-dev", "/bin/bash", "-c"]

RUN conda info --env && \
    python --version && \
    conda install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia -y && \
#    conda install -n base micromamba && \
    conda install \
        tqdm \
        iprogress \
        ffmpeg \
        ffmpeg-python \
        pillow -y && \
    conda install transformers -y && \
    conda install tensorflow -y && \
    conda install jupyterlab -y && \
    conda install \
        timm \
        datasets \
        evaluate \
        scikit-learn \
        pandas \
        peft \
        accelerate \
        autoawq \
        optimum \
        auto-gptq \
        bitsandbytes>=0.39.0 \
        jiwer \
        soundfile>=0.12.1 \
        librosa \
        langchain \
        gradio \
        -y && \
    conda clean -afy

WORKDIR /workspace

RUN rm -r /temp

