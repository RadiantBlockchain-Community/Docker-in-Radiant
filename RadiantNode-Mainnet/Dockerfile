# The Radiant Blockchain Developers
# The purpose of this image is to be able to host Radiant Node (RXD) and electrumx
# Build with: `docker build .`
# Public images at: https://hub.docker.com/u/radiantcommunity
# Thanks to Bacto for detecting bugs in the code

FROM ubuntu:22.04

LABEL maintainer="code@radiant4people.com"
LABEL version="1.3.0"
LABEL description="Docker image for radiantd node"

ARG DEBIAN_FRONTEND=nointeractive
RUN apt update

ENV PACKAGES="\
  build-essential \
  software-properties-common \
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
  cmake"

RUN apt update && apt install --no-install-recommends -y $PACKAGES  && \
    rm -rf /var/lib/apt/lists/* && \
    apt clean
 
# Install radiant-node v1.3.0
WORKDIR /root
RUN git clone --depth 1 --branch v1.3.0 https://github.com/radiantblockchain/radiant-node.git
RUN mkdir /root/radiant-node/build

WORKDIR /root/radiant-node/build
RUN cmake -GNinja .. -DBUILD_RADIANT_QT=OFF -DBUILD_RADIANT_WALLET=OFF
RUN ninja
RUN ninja install

# Remove radiant-node folder, not need more
RUN rm /root/radiant-node -R
WORKDIR /root

COPY radiantnode.sh /radiantnode.sh
RUN chmod 755 /radiantnode.sh

EXPOSE 7333
 
RUN mkdir -p /data

VOLUME /data

ENTRYPOINT ["/bin/sh", "-c" , "/radiantnode.sh"]
