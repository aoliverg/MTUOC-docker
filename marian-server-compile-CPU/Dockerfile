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

# Install intel mkl
RUN wget -qO- 'https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB' | apt-key add -
RUN echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list
RUN apt-get update && apt-get install -y  gcc-8 g++-8  cmake 
RUN apt-get update && apt-get install -y  cmake intel-mkl-64bit-2020.0-088
RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 60 --slave /usr/bin/g++ g++ /usr/bin/g++-8
ENV CXXFLAGS "$CXXFLAGS -std=c++14"

#eflomal

RUN pip3 install Cython
WORKDIR /root
RUN git clone https://github.com/robertostling/eflomal.git
WORKDIR /root/eflomal
RUN make
RUN make install
RUN python3 setup.py install

#custom boost
WORKDIR /root
RUN wget https://boostorg.jfrog.io/artifactory/main/release/1.67.0/source/boost_1_67_0.tar.gz
RUN tar zxvf boost_1_67_0.tar.gz
WORKDIR /root/boost_1_67_0
RUN ./bootstrap.sh
RUN ./b2 -j16 --prefix=$(pwd) --libdir=$(pwd)/lib64 --layout=system link=static install

WORKDIR /root
# Install Marian
ENV MARIANPATH /root/marian-dev
RUN git clone https://github.com/marian-nmt/marian-dev
WORKDIR $MARIANPATH
RUN mkdir -p build
WORKDIR $MARIANPATH/build
RUN cmake .. -DUSE_STATIC_LIBS=on -DBOOST_ROOT=./boost_1_67_0 -DCOMPILE_SERVER=on -DCOMPILE_CUDA=off -DUSE_SENTENCEPIECE=on -DCOMPILE_CPU=on && make -j4

WORKDIR /root

RUN cp ./marian-dev/build/marian-server .
RUN mkdir marian
RUN cp ./marian-dev/build/marian-* ./marian
RUN cp ./marian-dev/build/spm_* ./marian
RUN rm -rf ./marian-dev
RUN rm -rf ./eflomal
RUN rm boost_1_67_0.tar.gz
RUN rm -rf boost_1_67_0

