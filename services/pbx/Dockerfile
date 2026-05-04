FROM alpine:3.23

# Install necessary dependencies for Asterisk compilation
RUN apk add --no-cache \
    build-base \
    linux-headers \
    wget \
    ncurses-dev \
    util-linux-dev \
    jansson-dev \
    libxml2-dev \
    sqlite-dev \
    pkgconf \
    libedit-dev \
    openssl-dev

WORKDIR /usr/src/asterisk

# Copy the asterisk source folder
COPY ./source/asterisk /usr/src/asterisk

# If the folder is empty, these commands will fail, but assuming asterisk is cloned inside
RUN if [ -f configure ]; then \
    ./configure && \
    make menuselect.makeopts && \
    make && \
    make install && \
    make samples && \
    make config; \
    else echo "Asterisk source not found, skipping build"; fi

EXPOSE 5060/udp 5060/tcp
EXPOSE 10000-20000/udp

CMD ["asterisk", "-f"]
