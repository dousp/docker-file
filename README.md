# docker-file
docker file


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

### cuda镜像构建需要做处理

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