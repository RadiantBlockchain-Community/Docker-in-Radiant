# The Radiant Blockchain Developers
# The purpose of this image is to be able to host Radiant Node (RXD) and electrumx
# Build with: `docker build .`
# Public images at: https://hub.docker.com/u/radiantcommunity
# Thanks to Bacto for detecting bugs in the code

FROM ubuntu:22.04

LABEL maintainer="code@radiant4people.com"
LABEL version="1.2.0"
LABEL description="Docker image for radiantd node"

ARG DEBIAN_FRONTEND=nointeractive
RUN apt update
RUN apt-get install -y curl
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -
RUN apt-get install -y nodejs

ENV PACKAGES="\
  build-essential \
  libcurl4-openssl-dev \
  software-properties-common \
  ubuntu-drivers-common \
  pkg-config \
  libtool \
  openssh-server \
  git \
  clinfo \
  autoconf \
  automake \
  libjansson-dev \
  libevent-dev \
  uthash-dev \
  nodejs \
  vim \
  libboost-chrono-dev \
  libboost-filesystem-dev \
  libboost-test-dev \
  libboost-thread-dev \
  libevent-dev \
  libminiupnpc-dev \
  libssl-dev \
  libzmq3-dev \ 
  help2man \
  ninja-build \
  python3 \
  libdb++-dev \
  wget \
  cmake \
  ocl-icd-* \
  opencl-headers \
  ocl-icd-opencl-dev\
"
# Note can remove the opencl and ocl packages above when not building on a system for GPU/mining
# Included only for reference purposes if this container would be used for mining as well.

RUN apt update && apt install --no-install-recommends -y $PACKAGES  && \
    rm -rf /var/lib/apt/lists/* && \
    apt clean
 
# Install radiant-node
WORKDIR /root
RUN git clone --depth 1 --branch v1.2.0 https://github.com/radiantblockchain/radiant-node.git
RUN mkdir /root/radiant-node/build
WORKDIR /root/radiant-node/build
RUN cmake -GNinja .. -DBUILD_RADIANT_QT=OFF -DBUILD_RADIANT_WALLET=OFF
RUN ninja
RUN ninja install

# Create directory
RUN mkdir "/root/.radiant/"
RUN touch "/root/.radiant/radiant.conf"

RUN echo "rpcuser=$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 15 | head -n 1)" >>/root/.radiant/radiant.conf 
RUN echo "rpcpassword=$(cat /dev/urandom | tr -dc 'a-zA-Z0-9' | fold -w 20 | head -n 1)" >>/root/.radiant/radiant.conf 

RUN echo '\
\n\
listen=1\n\
daemon=0\n\
server=1\n\
rest=1\n\
rpcworkqueue=32\n\
rpcthreads=32\n\
rpcallowip=0.0.0.0/0\
' >>/root/.radiant/radiant.conf  

EXPOSE 7332 7333
 
VOLUME /root

ENTRYPOINT ["/bin/sh", "-c" , "radiantd"]
