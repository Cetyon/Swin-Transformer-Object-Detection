ARG PYTORCH="1.10.0"
ARG CUDA="11.3"
ARG CUDNN="8"
ARG CUDA_VERSION="113"
FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ARG MMCV="1.3.17"
ARG MMDET="2.20.0"

ENV PYTHONUNBUFFERED TRUE

RUN apt-key del 7fa2af80
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu2004/x86_64/7fa2af80.pub

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y \
    ca-certificates \
    g++ \
    openjdk-8-jdk \
    wget \
    # MMDet Requirements
    ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libsm6 libxrender-dev libxext6 \
    && rm -rf /var/lib/apt/lists/*

ENV PATH="/opt/conda/bin:$PATH"
RUN export FORCE_CUDA=1

# TORCHSEVER
RUN pip install torchserve torch-model-archiver

# MMLAB
ARG PYTORCH
ARG CUDA
ARG CUDA_VERSION
RUN ["/usr/bin/bash", "pip install mmcv-full==${MMCV} -f https://download.openmmlab.com/mmcv/dist/cu${CUDA_VERSION}/torch${PYTORCH}/index.html"]
RUN pip install mmdet==${MMDET}


RUN cd ~

# MMDETECTION
RUN git clone https://github.com/Cetyon/Swin-Transformer-Object-Detection.git mmdetection && \
    cd mmdetection && \
    pip install -r requirements/build.txt && \
    pip install -v -e . && \
    mkdir ./data

# APEX
RUN git clone https://github.com/NVIDIA/apex 
