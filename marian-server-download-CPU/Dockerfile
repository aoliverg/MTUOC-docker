FROM ubuntu:20.04

MAINTAINER Antoni Oliver <aoliverg@uoc.edu>
LABEL description="MTUOC-server with latest dev marian."

# Install necessary system packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -yq && apt-get install -yq \
                build-essential \
                git-core \
                pkg-config \
                libtool \
                zlib1g-dev \
                libbz2-dev \
                automake \
                python3-dev \
                perl \
                libsparsehash-dev \
                libboost-all-dev \
                libprotobuf17 \
                protobuf-compiler \
                libprotobuf-dev \
                openssl \
                libssl-dev \
                libgoogle-perftools-dev \
                wget \
                apt-transport-https ca-certificates gnupg software-properties-common \
                cmake \
                vim nano unzip gzip python3-pip php \
            && rm -rf /var/lib/apt/lists/*

WORKDIR /root
# Copy Marian
RUN wget http://lpg.uoc.edu/MTUOC/marian-server-cpu-v1.11.5/marian-server
RUN chmod +x marian-server

#MTUOC-server dependencies
RUN pip3 install subword_nmt \
    requests \
    XlsxWriter \
    xmltodict \
    lxml \
    Flask \
    nltk \
    sentencepiece \
    numpy \
    dicttoxml \
    PyYAML \
    SimpleWebSocketServer \
    websocket_client numpy

#eflomal (uncomment if needed)

#RUN pip3 install Cython
#WORKDIR /root
#RUN git clone https://github.com/robertostling/eflomal.git
#WORKDIR /root/eflomal
#RUN make
#RUN make install
#RUN python3 setup.py install
#WORKDIR /root
#RUN rm -rf ./eflomal

#Copying machine translation engine
RUN wget http://urloftheengine/engine.zip
RUN unzip engine.zip
RUN rm engine.zip
