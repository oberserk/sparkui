FROM ubuntu:20.04

###
# Set Python
ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHONUNBUFFERED=1

###
# Install OS Package
RUN apt-get update && \
    apt-get install -y python3.8 python3.8-dev python3-pip python3-setuptools && \
    pip3 --no-cache-dir install --upgrade pip && \
    ln -s /usr/bin/python3 /usr/bin/python && \
    apt-get install -y libaio1 unixodbc unixodbc-dev vim gcc wget unzip wget nginx curl ca-certificates && \
    apt-get install -y make libz-dev libssl-dev libffi-dev openssl && \
    rm -rf /var/lib/apt/lists/*

###
# Install Python Package
COPY requirements.txt /
RUN pip install -r /requirements.txt && \
    rm -rf /requirements.txt


###
# TimeZone
ENV TZ=Asia/Seoul
RUN ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime


###
# EntryPoint
#ENTRYPOINT ["python3", "/app/python_version.py"]


###
# Base files for Real-time Serving
COPY nginx.conf /etc/nginx/
COPY wsgi.py /usr/local/bin/
COPY predictor.py /usr/local/bin/
COPY serve /usr/local/bin/
RUN chmod 777 /usr/local/bin/serve

