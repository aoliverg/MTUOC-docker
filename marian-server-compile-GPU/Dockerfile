FROM nvidia/cuda:11.6.0-devel-ubuntu20.04

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
                nvidia-utils-470 \
            && rm -rf /var/lib/apt/lists/*
            


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

#eflomal

RUN pip3 install Cython
WORKDIR /root
RUN git clone https://github.com/robertostling/eflomal.git
WORKDIR /root/eflomal
RUN make
RUN make install
RUN python3 setup.py install

WORKDIR /root
# Install Marian
ENV MARIANPATH /root/marian-dev
RUN git clone https://github.com/marian-nmt/marian-dev
WORKDIR $MARIANPATH
RUN mkdir -p build
WORKDIR $MARIANPATH/build
RUN cmake .. -DCMAKE_BUILD_TYPE=Release -DUSE_STATIC_LIBS=on -DCOMPILE_SERVER=on -DUSE_SENTENCEPIECE=ON && make -j4

WORKDIR /root

RUN cp ./marian-dev/build/marian-server .
RUN mkdir marian
RUN cp ./marian-dev/build/marian-* ./marian
RUN cp ./marian-dev/build/spm_* ./marian
RUN rm -rf ./marian-dev
RUN rm -rf ./eflomal

