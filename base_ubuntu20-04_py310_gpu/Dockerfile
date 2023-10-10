FROM nvidia/cuda:11.4.3-cudnn8-devel-ubuntu20.04
#FROM ubuntu:20.04

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
    apt-get install -y make libz-dev libssl-dev libffi-dev openssl libbz2-dev && \
    apt-get upgrade -y linux-libc-dev && \
    rm -rf /var/lib/apt/lists/*

###
# Install Python 3.10

RUN mkdir -p /tmp
WORKDIR /tmp

RUN wget https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz && \
    tar xzvf Python-3.10.13.tgz  && \
    cd Python-3.10.13 && \
    ./configure --with-ssl && \
    #make && \
    #make install && \
    make altinstall && \
    cd .. && rm -rf Python-3.10.13

RUN rm -rf /usr/bin/python && \
    rm -rf /usr/bin/python3 && \
    ln -s /usr/local/bin/python3.10 /usr/bin/python && \
    ln -s /usr/local/bin/python3.10 /usr/bin/python3

RUN wget https://bootstrap.pypa.io/get-pip.py && \
    python get-pip.py && \
    python -m pip install --user --upgrade pip && \
    python -m pip install Cython



WORKDIR /


###
# CUDNN
ENV NV_CUDNN_VERSION 8.2.4.15
ENV NV_CUDNN_PACKAGE "libcudnn8=$NV_CUDNN_VERSION-1+cuda11.4"
ENV NV_CUDNN_PACKAGE_DEV "libcudnn8-dev=$NV_CUDNN_VERSION-1+cuda11.4"
ENV NV_CUDNN_PACKAGE_NAME "libcudnn8"

###
# Install OS Package
RUN apt-get install -y --no-install-recommends ${NV_CUDNN_PACKAGE} ${NV_CUDNN_PACKAGE_DEV} && \
    apt-mark hold ${NV_CUDNN_PACKAGE_NAME} && \
    rm -rf /var/lib/apt/lists/*

###
# Install Python Package
#RUN pip install --index-url=http://10.81.121.104:13086/simple/ --trusted-host 10.81.121.104 -r /requirements.txt && \
RUN pip install cryptography -U

COPY requirements.txt /
RUN pip install -r /requirements.txt && \
    rm -rf /requirements.txt


###
# TimeZone
ENV TZ=Asia/Seoul
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime


###
# PIP
#RUN mkdir -p /root/.pip
#COPY pip.conf /root/.pip/


###
# EntryPoint
#ENTRYPOINT ["python3", "/app/python_version.py"]

# Set up the program in the image to transfrom
COPY serve/nginx.conf /etc/nginx/
COPY serve/wsgi.py /usr/local/bin/
COPY serve/predictor.py /usr/local/bin/
COPY serve/serve /usr/local/bin/
RUN chmod 777 /usr/local/bin/serve

COPY _mapping.py /usr/local/lib/python3.10/site-packages/sagemaker_containers/

