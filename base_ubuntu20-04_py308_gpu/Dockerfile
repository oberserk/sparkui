FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu20.04

ENV PY_VERSION 3.8.18

###
# Set Python
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1

###
# Install OS Package
RUN apt-get update && \
    apt-get -y purge python3.8 && \
    apt-get -y autoremove && \
    apt-get install -y libaio1 unixodbc unixodbc-dev vim gcc wget unzip wget nginx ca-certificates && \
    apt-get install -y make libz-dev libssl-dev libffi-dev openssl libbz2-dev libsqlite3-dev && \
    apt-get upgrade -y linux-libc-dev && \
    rm -rf /var/lib/apt/lists/*

###
# Install Python

RUN mkdir -p /tmp
WORKDIR /tmp

RUN wget https://www.python.org/ftp/python/${PY_VERSION}/Python-${PY_VERSION}.tgz && \
    tar xzvf Python-${PY_VERSION}.tgz  && \
    cd Python-${PY_VERSION} && \
    ./configure --with-ssl && \
    make altinstall && \
    cd .. && rm -rf Python-${PY_VERSION}

RUN rm -rf /usr/bin/python && \
    rm -rf /usr/bin/python3 && \
    ln -s /usr/local/bin/python3.8 /usr/bin/python && \
    ln -s /usr/local/bin/python3.8 /usr/bin/python3

RUN wget https://bootstrap.pypa.io/get-pip.py && \
    python get-pip.py && \
    python -m pip install --user --upgrade pip && \
    python -m pip install Cython



WORKDIR /


###
# CUDNN
ENV NV_CUDNN_VERSION 8.6.0.163
ENV NV_CUDNN_PACKAGE "libcudnn8"
ENV NV_CUDNN_PACKAGE_DEV "libcudnn8-dev"
ENV NV_CUDNN_PACKAGE_NAME "libcudnn8"

###
# Install OS Package
RUN apt-get install -y --no-install-recommends ${NV_CUDNN_PACKAGE} ${NV_CUDNN_PACKAGE_DEV} && \
    apt-mark hold ${NV_CUDNN_PACKAGE_NAME} && \
    rm -rf /var/lib/apt/lists/*

###
# Install Python Package
RUN pip install cryptography -U
RUN pip install setuptools -U

COPY requirements.txt /
RUN pip install -r /requirements.txt && \
    rm -rf /requirements.txt


###
# TimeZone
ENV TZ=Asia/Seoul
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime


###
# Hosting Inference
COPY serve/nginx.conf /etc/nginx/
COPY serve/wsgi.py /usr/local/bin/
COPY serve/predictor.py /usr/local/bin/
COPY serve/serve /usr/local/bin/
RUN chmod 777 /usr/local/bin/serve

