FROM alpine

LABEL maintainer="ZHUANGZHUANG <mail@zhuangzhuang.ml>"

ARG SNELL_VERSION="v2.0.3"

RUN SNELL_URL="https://github.com/surge-networks/snell/releases/download" && \
    SNELL_FILENAME="snell-server-${SNELL_VERSION}-linux-amd64.zip" && \
    ALPINE_GLIBC_PACKAGE_VERSION="2.29-r0" && \
    ALPINE_GLIBC_BASE_URL="https://github.com/sgerrand/alpine-pkg-glibc/releases/download" && \
    ALPINE_GLIBC_BASE_PACKAGE_FILENAME="glibc-$ALPINE_GLIBC_PACKAGE_VERSION.apk" && \
    ALPINE_GLIBC_BIN_PACKAGE_FILENAME="glibc-bin-$ALPINE_GLIBC_PACKAGE_VERSION.apk" && \
    apk add --no-cache --virtual .build-deps wget ca-certificates && \
    echo \
        "-----BEGIN PUBLIC KEY-----\
        MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEApZ2u1KJKUu/fW4A25y9m\
        y70AGEa/J3Wi5ibNVGNn1gT1r0VfgeWd0pUybS4UmcHdiNzxJPgoWQhV2SSW1JYu\
        tOqKZF5QSN6X937PTUpNBjUvLtTQ1ve1fp39uf/lEXPpFpOPL88LKnDBgbh7wkCp\
        m2KzLVGChf83MS0ShL6G9EQIAUxLm99VpgRjwqTQ/KfzGtpke1wqws4au0Ab4qPY\
        KXvMLSPLUp7cfulWvhmZSegr5AdhNw5KNizPqCJT8ZrGvgHypXyiFvvAH5YRtSsc\
        Zvo9GI2e2MaZyo9/lvb+LbLEJZKEQckqRj4P26gmASrZEPStwc+yqy1ShHLA0j6m\
        1QIDAQAB\
        -----END PUBLIC KEY-----" | sed 's/   */\n/g' > "/etc/apk/keys/sgerrand.rsa.pub" && \
    wget --no-cache \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BASE_URL/$ALPINE_GLIBC_PACKAGE_VERSION/$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$SNELL_URL/$SNELL_VERSION/$SNELL_FILENAME" && \
    apk add --no-cache libstdc++ \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" && \
    apk del .build-deps && \
    unzip "$SNELL_FILENAME" -d /usr/sbin/ && \
    rm  -rf \
        "/etc/apk/keys/sgerrand.rsa.pub" \
        "/root/.wget-hsts" \
        "$ALPINE_GLIBC_BASE_PACKAGE_FILENAME" \
        "$ALPINE_GLIBC_BIN_PACKAGE_FILENAME" \
        "$SNELL_FILENAME"

ENTRYPOINT ["/usr/sbin/snell-server", "-c"]
CMD ["/etc/snell/snell.conf"]

# [snell-server]
# listen = 0.0.0.0:13254
# psk = G16SntvtSQ5Jo5uebDrlD49BZzbNTLF
# obfs = tls

# docker run -d -p 13254:13254/tcp -p 13254:13524/udp -v /etc/snell/snell.conf:/etc/snell/snell.conf ydzydzydz/snell:latest
