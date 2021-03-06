FROM alpine:latest as builder
LABEL maintainer="dev@ebon.network"
LABEL version="3.3.4"
RUN apk add --no-cache \
    asciidoc \
    autoconf \
    automake \
    build-base \
    c-ares-dev \
    git \
    libev-dev \
    libtool \
    libsodium-dev \
    linux-headers \
    mbedtls-dev \
    pcre-dev \
    xmlto
RUN git clone https://github.com/shadowsocks/shadowsocks-libev.git \
    && cd shadowsocks-libev \
    && git submodule update --init --recursive \
    && ./autogen.sh \
    && ./configure \
    && make

FROM alpine:latest
COPY --from=builder /shadowsocks-libev/src/ss-server /usr/bin/ss-server
RUN apk add --no-cache libev \
    libsodium \
    mbedtls \
    pcre \
    c-ares \
    haveged \
    && mkdir /etc/shadowsocks-libev
WORKDIR /etc/shadowsocks-libev
CMD [ "ss-server", "-c", "/etc/shadowsocks-libev/config.json" ]