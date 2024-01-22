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
    pip3 install --no-cache-dir --yes transformers &&\
    pip3 install --no-cache-dir --yes ffmpeg &&\
    pip3 install --no-cache-dir --yes ffmpeg-python &&\
    pip3 install --no-cache-dir --yes timm &&\
    pip3 install --no-cache-dir --yes datasets &&\
    pip3 install --no-cache-dir --yes evaluate &&\
    pip3 install --no-cache-dir --yes scikit-learn &&\
    pip3 install --no-cache-dir --yes pandas &&\
    pip3 install --no-cache-dir --yes peft &&\
    pip3 install --no-cache-dir --yes accelerate &&\
    pip3 install --no-cache-dir --yes autoawq &&\
    pip3 install --no-cache-dir --yes optimum &&\
    pip3 install --no-cache-dir --yes auto-gptq &&\
    pip3 install --no-cache-dir --yes bitsandbytes>=0.39.0 &&\
    pip3 install --no-cache-dir --yes jiwer &&\
    pip3 install --no-cache-dir --yes soundfile>=0.12.1 &&\
    pip3 install --no-cache-dir --yes librosa &&\
    pip3 install --no-cache-dir --yes langchain &&\
    pip3 install --no-cache-dir --yes gradio &&\
    pip cache purge