FROM nvidia/cuda:12.1.1-cudnn8-runtime-ubuntu22.04

# 指定使用bash
#RUN rm /bin/sh && ln -s /bin/bash /bin/sh
SHELL ["/bin/bash", "-c"]

# 设置环境变量
ENV \
    PYTHON_VERSION=3.11.7 \
    # 非交互方式安装包
    DEBIAN_FRONTEND=noninteractive \
    TZ=Asia/Shanghai \
    PATH="/opt/miniconda/bin:$PATH"

WORKDIR /temp

# 安装基础包
RUN \
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
    apt-get install -y tzdata && ln -snf /usr/share/zoneinfo/"$TZ" /etc/localtime && echo "$TZ" > /etc/timezone &&\
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 安装Miniconda、micromamba
# 创建llm-dev环境并安装PyTorch
#ENV CONDA_DEFAULT_ENV llm-dev
RUN \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/miniconda  && \
    conda config --add channels bioconda && \
    conda config --add channels anaconda && \
    conda config --add channels nvidia && \
    conda config --add channels pytorch && \
    conda config --add channels conda-forge && \
    conda config --set show_channel_urls yes && \
    conda clean -i && \
    conda update conda -y && \
    conda install -n base micromamba -y && \
    conda create -n llm-dev python="$PYTHON_VERSION" && \
    echo "alias mba='micromamba'" >> ~/.bashrc && \
    echo "alias pip='pip3'" >> ~/.bashrc && \
    echo "alias python='python3'" >> ~/.bashrc && \
    echo ". /opt/miniconda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate llm-dev" >> ~/.bashrc

#ENV PATH /opt/miniconda/envs/llm-dev/bin:$PATH

SHELL ["conda", "run", "--no-capture-output", "-n", "llm-dev", "/bin/bash", "-c"]

RUN conda info --env && \
    python --version && \
    micromamba install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia -y && \
    micromamba install tensorflow -y && \
    micromamba install transformers -y && \
    micromamba install jupyterlab -y && \
    conda clean -afy && \
    micromamba clean -afy && \
    pip3 install \
        tqdm \
        iprogress \
        ffmpeg \
        ffmpeg-python \
        pillow \
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
        gradio && \
#    pip3 install jupyter_contrib_nbextensions && \
#    pip3 install jupyter_nbextensions_configurator && \
#    jupyter contrib nbextension install --user && \
#    jupyter nbextensions_configurator enable --user && \
    pip3 cache purge && \
    jupyter notebook --generate-config && \
    echo "c.NotebookApp.token = 'ddd'"  >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.port = 8888"  >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.NotebookApp.ip = '0.0.0.0'" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.ServerApp.open_browser = True" >> ~/.jupyter/jupyter_notebook_config.py && \
    echo "c.ServerApp.allow_root = True" >> ~/.jupyter/jupyter_notebook_config.py

WORKDIR /workspace

RUN rm -r /temp

CMD ["sh", "-c", "nohup jupyter notebook --allow-root --no-browser > /tmp/jupyter.log 2>&1 &"]

EXPOSE 8888

