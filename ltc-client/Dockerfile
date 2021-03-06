FROM ubuntu:18.04 as builder

ADD docker/Makefile /tmp/Makefile
RUN apt-get update && \
    apt-get install --no-install-recommends -y \
        automake \
        autotools-dev \
        binutils \
        bsdmainutils \
        build-essential \
        ca-certificates \
        git \
        libboost-all-dev \
        libevent-dev \
        libminiupnpc-dev \
        libprotobuf-dev \
        libssl-dev \
        libtool \
        pkg-config \
        wget && \
    cd /tmp && \
    make install && \
    strip /usr/local/bin/*litecoin*

FROM ubuntu:18.04

COPY --from=builder /usr/local/bin/*litecoin* /usr/local/bin/

RUN useradd -r -u 10000 dockeruser && \
    mkdir -p /opt/litecoin/data && \
    chown -R dockeruser /opt/litecoin && \
    # packages
    apt-get update && \
    apt-get install --no-install-recommends -y \
        libboost-chrono1.65.1 \
        libboost-filesystem1.65.1 \
        libboost-program-options1.65.1 \
        libboost-system1.65.1 \
        libboost-thread1.65.1 \
        libevent-2.1-6 \
        libevent-pthreads-2.1-6 \
        libminiupnpc10 \
        libssl1.1

ADD docker/litecoin.conf /opt/litecoin/litecoin.conf

USER dockeruser
EXPOSE 8532

CMD bash
CMD litecoind -conf=/opt/litecoin/litecoin.conf -datadir=/opt/litecoin/data -daemon -rest && bash
