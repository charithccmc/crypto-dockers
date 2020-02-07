FROM alpine:3.8.4

RUN apk --no-cache add make bash boost boost-program_options libevent libressl shadow && \
    useradd -r -u 10000 dockeruser &&  \
    mkdir -p /opt/bitcoin-cash/data && \
    chown -R dockeruser /opt/bitcoin-cash

ADD docker/bitcoin.conf /opt/bitcoin-cash/bitcoin.conf
ADD docker/Makefile /tmp/Makefile

RUN apk --no-cache --virtual build-dependendencies add \
        linux-headers \
        libressl-dev \
        g++ \
        boost-dev \
        file \
        autoconf \
        automake \
        libtool \
        libevent-dev \
        git \
        coreutils \
        binutils \
        grep && \
    cd /tmp; make install && \
    rm -rf /tmp/src && \
    strip /usr/local/bin/*bitcoin* && \
    apk del build-dependendencies

USER dockeruser
EXPOSE 8432

CMD bitcoind -conf=/opt/bitcoin-cash/bitcoin.conf -datadir=/opt/bitcoin-cash/data -daemon -rest && bash