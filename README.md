# 踩坑记录

##  Nvidia Container Toolkit

- 不安这个容器没办法用宿主机显卡
- 安装：https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html

## CUDA
 
- N卡必备
- 安装：https://developer.nvidia.com/cuda-12-1-1-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=deb_network
- docker镜像：https://hub.docker.com/r/nvidia/cuda/tags

## PyTorch

- 官网/安装：https://pytorch.org/
- docker镜像：https://hub.docker.com/r/pytorch/pytorch/tags

## Jupyter Notebook


## Install Python
- 安装conda会自带python，这个就省了

```dockerfile
# https://www.python.org/ftp/python/
RUN wget https://www.python.org/ftp/python/"$PYTHON_VERSION"/Python-"$PYTHON_VERSION".tgz && \
    tar -xvf Python-"$PYTHON_VERSION".tgz && \
    cd Python-"$PYTHON_VERSION" && \
    ./configure --enable-optimizations && \
    make && \
    make install && \
    ln -s /usr/local/bin/python3 /usr/local/bin/python && \
    ln -s /usr/local/bin/pip3 /usr/local/bin/pip
```

## 问题整理

### conda无法激活创建的环境

- 通过`SHELL`命令处理

```dockerfile
ENV PATH="/opt/miniconda/bin:$PATH"

RUN \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -p /opt/miniconda  && \
    conda update -y conda && \
    conda create -n llm-dev python="$PYTHON_VERSION"

SHELL ["conda", "run", "--no-capture-output", "-n", "llm-dev", "/bin/bash", "-c"]

```

### conda默认没有`conda-forge`
conda config --append channels conda-forge


### cuda镜像构建需要做处理
- 用cuda镜像的时候容器出幺蛾子
- https://github.com/NVIDIA/nvidia-container-toolkit/issues/258

```dockerfile
RUN \
    rm -f /etc/apt/sources.list.d/cuda*.list && \
    apt-key del 7fa2af80 && \
    apt-get update && \
    apt-get clean

RUN \
    apt-get install -y --no-install-recommends sudo wget && \
    wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb && \
    sudo dpkg -i cuda-keyring_1.0-1_all.deb
```

## 帖子参考

- https://zhuanlan.zhihu.com/p/471484611 
  - 这个作者还提供了能生成构建脚本的github repo
  - https://github.com/cnstark/pytorch-docker
