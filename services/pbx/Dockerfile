FROM debian:bookworm-slim AS builder

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    file \
    wget \
    curl \
    git \
    subversion \
    pkg-config \
    autoconf \
    automake \
    libtool \
    bison \
    flex \
    uuid-dev \
    libjansson-dev \
    libxml2-dev \
    libsqlite3-dev \
    libssl-dev \
    libncurses5-dev \
    libedit-dev \
    libsrtp2-dev \
    unixodbc-dev \
    libnewt-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /build
COPY ./source/asterisk ./asterisk

WORKDIR /build/asterisk

# Build Asterisk certified-22.8-cert2 from local source.
# Use bundled pjproject for the version expected by Asterisk.
RUN ./configure --with-pjproject-bundled \
    && make menuselect.makeopts \
    && menuselect/menuselect --disable BUILD_NATIVE menuselect.makeopts \
    && make -j$(nproc) \
    && make install \
    && make samples

FROM debian:bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    libjansson4 \
    libxml2 \
    libsqlite3-0 \
    libssl3 \
    libncurses6 \
    libedit2 \
    libsrtp2-1 \
    unixodbc \
    libnewt0.52 \
    && rm -rf /var/lib/apt/lists/*

RUN groupadd --system asterisk && \
    useradd --system --gid asterisk --home-dir /var/lib/asterisk asterisk

COPY --from=builder /usr/sbin/asterisk /usr/sbin/asterisk
COPY --from=builder /usr/lib/asterisk /usr/lib/asterisk
COPY --from=builder /usr/lib/libasterisk*.so* /usr/lib/
COPY --from=builder /etc/asterisk /etc/asterisk
COPY --from=builder /var/lib/asterisk /var/lib/asterisk
COPY --from=builder /var/spool/asterisk /var/spool/asterisk

RUN mkdir -p /var/log/asterisk /var/run/asterisk && \
    chown -R asterisk:asterisk \
        /etc/asterisk \
        /var/lib/asterisk \
        /var/spool/asterisk \
        /var/log/asterisk \
        /var/run/asterisk

EXPOSE 5060/udp 5060/tcp 10000-20000/udp

CMD ["/usr/sbin/asterisk", "-f", "-U", "asterisk", "-G", "asterisk"]