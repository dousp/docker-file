FROM pytorch/pytorch:2.1.2-cuda12.1-cudnn8-runtime

LABEL maintainer="DD <douspeng@sina.cn>" \
      version="1.0" \
      description="base env"

ENV TZ=Asia/Shanghai

RUN cp /etc/apt/sources.list /etc/apt/sources.list.bak &&\
    chmod a+x /etc/apt/sources.list  &&\
    sed -i '1,$d' /etc/apt/sources.list &&\
    echo "# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释" >> /etc/apt/sources.list  &&\
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "# 预发布软件源，不建议启用" >> /etc/apt/sources.list  &&\
    echo "# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    echo "# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jjammy-proposed main restricted universe multiverse" >> /etc/apt/sources.list  &&\
    apt-get update && apt-get upgrade -y

RUN apt-get install tzdata -y --no-install-recommends && ln -snf /usr/share/zoneinfo/"$TZ" /etc/localtime && echo "$TZ" > /etc/timezone &&\
    apt-get install sudo gcc make cron dialog openssh-server openssl -y --no-install-recommends &&\
#    apt-get install libssl-dev libsm6 libxext6 -y --no-install-recommends &&\
    apt-get install vim git python3-pip -y --no-install-recommends &&\
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/'  /etc/ssh/sshd_config &&\
    echo  "alias ll='ls -l'" >> ~/.bash_profile &&\
    echo  "alias pip='pip3'" >> ~/.bash_profile &&\
    pip3 config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple &&\
    pip install pip -U &&\
    pip3 install --no-cache-dir -y -i https://pypi.tuna.tsinghua.edu.cn/simple \
        transformers \
        ffmpeg \
        ffmpeg-python \
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
        gradio &&\
    pip cache purge