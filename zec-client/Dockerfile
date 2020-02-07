FROM ubuntu:18.04 as builder

RUN apt-get update && \
    apt-get install --no-install-recommends -y \
        automake \
        autoconf \
        autotools-dev \
        binutils \
        bsdmainutils \
        build-essential \
        ca-certificates \
        curl \
        git \
        libevent-dev \
        libgomp1 \
        libssl-dev \
        libtool \
        m4 \
        pkg-config \
        procps \
        unzip \
        wget \
        zlib1g-dev

ADD docker/Makefile /tmp/Makefile
RUN cd /tmp && \
    make install && \
    cd / && \
    rm -rf /tmp/src && \
    apt-get autoremove --purge -y \
        autoconf \
        automake \
        binutils \
        build-essential \
        gcc \
        git \
        libevent-dev \
        libgcc-6-dev \
        libssl-dev \
        perl \
        zlib1g-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

FROM ubuntu:18.04

COPY --from=builder /usr/local/bin/zcash* /usr/local/bin/
COPY --from=builder /root/.zcash-params /home/dockeruser/.zcash-params

ADD docker/zcash.conf /opt/zcash/zcash.conf

RUN apt-get update && \
    apt-get install --no-install-recommends -y libgomp1 && \
    useradd -r -u 10000 dockeruser && \
    mkdir -p /opt/zcash/data && \
    chown dockeruser -R /opt/zcash

USER dockeruser
EXPOSE 8632

CMD zcashd -conf=/opt/zcash/zcash.conf -datadir=/opt/zcash/data -daemon -rest && bash
