FROM pytorch/pytorch:2.1.2-cuda12.1-cudnn8-runtime

LABEL maintainer="DD <douspeng@sina.cn>" \
      version="1.0" \
      description="base env"

ENV TZ=Asia/Shanghai
ENV DEBIAN_FRONTEND noninteractive

RUN rm -rf /var/lib/apt/lists/* && apt-get clean && apt-get update && apt-get upgrade -y &&\
#    apt-get install gcc make cron dialog libssl-dev libsm6 libxext6 openssh-server openssl -y --no-install-recommends &&\
    apt-get install -y --no-install-recommends apt-utils &&\
    apt-get install -y --no-install-recommends software-properties-common &&\
    apt-get install -y --no-install-recommends sudo curl wget git vim unzip zip python3-pip python3-dev build-essential &&\
    apt-get install tzdata -y && ln -snf /usr/share/zoneinfo/"$TZ" /etc/localtime && echo "$TZ" > /etc/timezone &&\
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN echo  "alias ll='ls -l'" >> ~/.bash_profile &&\
    echo  "alias pip='pip3'" >> ~/.bash_profile &&\
    pip install pip -U &&\
    pip install --no-cache-dir  transformers --yes &&\
    pip install --no-cache-dir ffmpeg --yes &&\
    pip install --no-cache-dir ffmpeg-python --yes &&\
    pip install --no-cache-dir timm --yes &&\
    pip install --no-cache-dir datasets --yes &&\
    pip install --no-cache-dir evaluate --yes &&\
    pip install --no-cache-dir scikit-learn --yes &&\
    pip install --no-cache-dir pandas --yes &&\
    pip install --no-cache-dir peft --yes &&\
    pip install --no-cache-dir accelerate --yes &&\
    pip install --no-cache-dir autoawq --yes &&\
    pip install --no-cache-dir optimum --yes &&\
    pip install --no-cache-dir auto-gptq --yes &&\
    pip install --no-cache-dir bitsandbytes>=0.39.0 --yes &&\
    pip install --no-cache-dir jiwer --yes &&\
    pip install --no-cache-dir soundfile>=0.12.1 --yes &&\
    pip install --no-cache-dir librosa --yes &&\
    pip install --no-cache-dir langchain --yes &&\
    pip install --no-cache-dir gradio --yes &&\
    pip cache purge