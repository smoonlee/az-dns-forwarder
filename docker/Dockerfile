FROM alpine:3.21

LABEL maintainer="github@builtwithcaffeine.cloud" \
org.label-schema.vcs-url="https://github.com/smoonlee/az-dns-forwarder"

ADD named.conf /etc/bind/

RUN apk add --no-cache bind && \
    rm -rf /var/cache/apk/*

EXPOSE 53/udp

ENTRYPOINT ["/usr/sbin/named", "-f", "-g"]
