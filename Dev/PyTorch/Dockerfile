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
    pip3 install --no-cache-dir -y transformers &&\
    pip3 install --no-cache-dir -y ffmpeg &&\
    pip3 install --no-cache-dir -y ffmpeg-python &&\
    pip3 install --no-cache-dir -y timm &&\
    pip3 install --no-cache-dir -y datasets &&\
    pip3 install --no-cache-dir -y evaluate &&\
    pip3 install --no-cache-dir -y scikit-learn &&\
    pip3 install --no-cache-dir -y pandas &&\
    pip3 install --no-cache-dir -y peft &&\
    pip3 install --no-cache-dir -y accelerate &&\
    pip3 install --no-cache-dir -y autoawq &&\
    pip3 install --no-cache-dir -y optimum &&\
    pip3 install --no-cache-dir -y auto-gptq &&\
    pip3 install --no-cache-dir -y bitsandbytes>=0.39.0 &&\
    pip3 install --no-cache-dir -y jiwer &&\
    pip3 install --no-cache-dir -y soundfile>=0.12.1 &&\
    pip3 install --no-cache-dir -y librosa &&\
    pip3 install --no-cache-dir -y langchain &&\
    pip3 install --no-cache-dir -y gradio &&\
    pip cache purge