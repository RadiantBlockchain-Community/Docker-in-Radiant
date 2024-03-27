# The Radiant Blockchain Developers
# The purpose of this image is to be able to host ElectrumX and radiantd (RXD) together.
# Build with: `docker build -t electrumx-node-testnet .`

FROM ubuntu:22.04

LABEL maintainer="code@radiant4people.com"
LABEL version="1.3.0"
LABEL description="Docker image for electrumx and radiantd node in testnet mode"

ARG DEBIAN_FRONTEND=nointeractive
RUN apt update

ENV PACKAGES="\
  build-essential \
  libcurl4-openssl-dev \
  software-properties-common \
  ubuntu-drivers-common \
  pkg-config \
  libtool \
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
  python3-pip \
  libdb++-dev \
  cmake \
"
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

####################################################### INSTALL ELECTRUMX WITH SSL

WORKDIR /root

# ORIGINAL SOURCE
RUN git clone --depth 1 --branch master https://github.com/radiantblockchain/electrumx.git

WORKDIR /root/electrumx

RUN python3 -m pip install -r requirements.txt

ENV DAEMON_URL=http://RadiantDockerUser:RadiantDockerPassword@localhost:17332/
ENV COIN=RadiantTestnet
ENV NET=testnet
ENV REQUEST_TIMEOUT=60
ENV DB_DIRECTORY=/data/radiantnode_testnet_electrumx/electrumdb
ENV DB_ENGINE=leveldb
ENV SERVICES=tcp://0.0.0.0:51010,ssl://0.0.0.0:51012,rpc://0.0.0.0:8000
ENV SSL_CERTFILE=/data/radiantnode_testnet_electrumx/electrumdb/server.crt
ENV SSL_KEYFILE=/data/radiantnode_testnet_electrumx/electrumdb/server.key
ENV HOST=""
ENV ALLOW_ROOT=true
ENV CACHE_MB=1500
ENV MAX_SESSIONS=5000
ENV MAX_SEND=10000000
ENV MAX_RECV=10000000
ENV COST_SOFT_LIMIT=0
ENV COST_HARD_LIMIT=0

EXPOSE 17333 51010 51012

COPY radiantnode_electrumx_testnet.sh /radiantnode_electrumx_testnet.sh

RUN chmod 755 /radiantnode_electrumx_testnet.sh

RUN mkdir -p /data

VOLUME /data

ENTRYPOINT ["/bin/sh", "-c" , "/radiantnode_electrumx_testnet.sh"]